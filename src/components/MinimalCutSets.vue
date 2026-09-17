<script setup>
import { ref } from 'vue';
import { useVisuallyJsUpdate } from "@visuallyjs/browser-ui-vue";
import { computeCutSets } from "../cut-sets.ts";

const minimalCutSets = ref([]);

useVisuallyJsUpdate((model) => {
    minimalCutSets.value = computeCutSets(model);
})
</script>

<template>
    <div class="minimal-cut-sets">
        <template v-if="minimalCutSets.length === 0">
            <p class="vjs-fta-inspector-empty">No cut sets found.</p>
        </template>
        <template v-else>
            <div class="vjs-fta-cut-sets-list">
                <div v-for="(set, i) in minimalCutSets" :key="i" class="vjs-fta-cut-set">
                    <div class="vjs-fta-cut-set-index">#{{ i + 1 }}</div>
                    <div class="vjs-fta-cut-set-events">
                        <span v-for="be in set" :key="be.id" class="vjs-fta-cut-set-event">
                            {{ be.label || be.id }}
                        </span>
                    </div>
                </div>
            </div>
        </template>
        <p class="vjs-fta-cut-sets-note"><i>Note: This is a static, combinatorial analysis. It does not account for event sequence or timing.</i></p>
    </div>
</template>
