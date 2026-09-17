<script setup>
import { ref } from 'vue';
import { ColumnChartComponent, useVisuallyJsUpdate } from "@visuallyjs/browser-ui-vue";
import { computeCutSets } from "../cut-sets.ts";

const chartData = ref([])

const chartOptions = {
    series:[
        {
            valueField:"value",
            label:"Importance"
        }
    ],
    valueAxis: {
        labelFormatter: (value) => {
            return `${value.toFixed(1)}`;
        }
    },
    categoryAxis: {
        title: {
            text: "Basic Events"
        },
        labelField:"label"
    }
}

useVisuallyJsUpdate((model) => {
    const minimalCutSets = computeCutSets(model)

    // Fussell-Vesely Importance Calculation
    const mcsWithProbs = minimalCutSets.map(mcs => {
        const prob = mcs.reduce((p, event) => p * (event.probability || 0), 1);
        return { events: mcs, probability: prob };
    });

    const topEventProbability = mcsWithProbs.reduce((sum, mcs) => sum + mcs.probability, 0);

    if (topEventProbability === 0) {
        chartData.value = [];
        return;
    }

    const basicEvents = model.getNodes().filter(n => n.type === 'basic-event').map(n => n.data);

    chartData.value = basicEvents.map(be => {
        const relevantMcs = mcsWithProbs.filter(mcs => mcs.events.some(event => event.id === be.id));
        const sumProb = relevantMcs.reduce((sum, mcs) => sum + mcs.probability, 0);
        const fvImportance = sumProb / topEventProbability;
        return {
            label: be.label || be.id,
            value: fvImportance
        };
    });
})
</script>

<template>
    <div class="vjs-fta-risk-contribution">
        <template v-if="chartData.length === 0">
            <p class="vjs-fta-inspector-empty">No Basic Events with probabilities found.</p>
        </template>
        <template v-else>
            <div>
                <ColumnChartComponent style="height: 300px; width: 100%;"
                    :data="chartData"
                    :options="chartOptions"
                />
            </div>
        </template>
    </div>
</template>
