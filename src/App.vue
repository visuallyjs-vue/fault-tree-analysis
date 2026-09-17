<script setup>
import { 
    DiagramComponent, 
    DiagramPaletteComponent, 
    DiagramProvider,
    ControlsComponent
} from "@visuallyjs/browser-ui-vue";
import diagramOptions from "./diagram-options"
import modelOptions from "./model-options.ts";
import Inspector from "./components/Inspector.vue";
import MinimalCutSets from "./components/MinimalCutSets.vue";
import RiskContributionChart from "./components/RiskContributionChart.vue";

const props = defineProps(['url'])
</script>

<template>
    <DiagramProvider>
        <div class="vjs-fta-container">
            <div class="vjs-fta-left-palette">
                <div style="padding: 15px; border-bottom: 1px solid #ccc; font-weight: bold;">FTA Symbols</div>
                <DiagramPaletteComponent :allowDropOnEdge="true" :showLabels="true" />
            </div>

            <div class="vjs-fta-center-canvas">
                <DiagramComponent :url="url" :options="diagramOptions" :modelOptions="modelOptions">
                    <ControlsComponent />
                </DiagramComponent>
            </div>

            <div class="vjs-fta-right-sidebar">
                <div class="vjs-fta-sidebar-content">
                    <div class="vjs-fta-sidebar-section">
                        <div class="vjs-fta-sidebar-section-header">Inspector</div>
                        <Inspector />
                    </div>
                    <div class="vjs-fta-sidebar-section">
                        <div class="vjs-fta-sidebar-section-header">Cut Sets</div>
                        <MinimalCutSets />
                    </div>
                    <div class="vjs-fta-sidebar-section">
                        <div class="vjs-fta-sidebar-section-header">Risk (Fussell-Vesely)</div>
                        <RiskContributionChart />
                    </div>
                </div>
            </div>
        </div>
    </DiagramProvider>
</template>
