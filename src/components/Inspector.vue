<script setup>
import { Node } from "@visuallyjs/browser-ui"
import { InspectorComponent } from "@visuallyjs/browser-ui-vue";
import {ref} from "vue";

const current = ref(null)
</script>

<template>
    <InspectorComponent v-model="current">
        <template v-if="current != null">
            <div class="vjs-fta-inspector-container">
                <template v-if="current.objectType === Node.objectType">
                    <div class="vjs-fta-inspector-group">
                        <label class="vjs-fta-inspector-label">Label: </label>
                        <input 
                            type="text" 
                            class="vjs-fta-inspector-input"
                            vjs-att="label"
                            vjs-focus="true"
                        />
                    </div>
                    
                    <template v-if="current.type === 'basic-event'">
                        <div class="vjs-fta-inspector-group">
                            <label class="vjs-fta-inspector-label">Probability (0-1): </label>
                            <input 
                                type="number" 
                                class="vjs-fta-inspector-input"
                                step="0.01" 
                                min="0" 
                                max="1" 
                                vjs-att="probability"
                                vjs-datatype="float"
                            />
                        </div>
                    </template>
                    
                    <div class="vjs-fta-inspector-footer">
                        ID: {{ current.getFullId() }}<br/>
                        Type: {{ current.type }}
                    </div>
                </template>
                <template v-else>
                    <div class="vjs-fta-inspector-footer">
                        ID: {{ current.getFullId() }}<br/>
                        Type: {{ current.objectType }}
                    </div>
                </template>
            </div>
        </template>
        <template v-else>
            <div class="vjs-fta-inspector-empty">Select a node to edit its properties.</div>
        </template>
    </InspectorComponent>
</template>
