<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
    options: Array,
    state: Object,
    trackBy: {type: String,default: 'id'}
})

const selectedOption  = computed(() => {
    /**
     * This relates to the selected `option` in the Select component.
     * Find if any of the `options` is selected according to the `state`
     * */

    // During recursion:
    // On the first iteration state.field should be used to find the selected option
    // On the second iteration: state.operator.
    // It seems that on each iteration the `option` needs to be aware of which `field` to use
    // Let's solve that by requiring every `option` object to have a `trackBy` property
    return props.options?.find(option => option.id === props.state[props.trackBy])
})

</script>

<template>
    <select v-model="state[trackBy]">
        <option v-for="option in options" :key="option.label" :value="option.id">{{ option.label }}</option>
    </select>

    <!-- If selected has options, recursively call this component  -->
    <VSelect v-if="selectedOption?.options" :options="selectedOption.options" :state="state" :trackBy="selectedOption.trackBy" />

    <!-- Else, render any existing children -->
    <div v-else-if="selectedOption?.children">
        <div v-for="(child, idx) in selectedOption.children" :key="idx">
            <!-- Type: Date -->
            <input v-if="child.type == 'date'" v-model="state[child.trackBy]" type="date" :id="child.id">
        </div>
    </div>

</template>


<style lang="scss" scoped>
</style>