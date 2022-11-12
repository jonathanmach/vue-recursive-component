<script setup>
import { ref, computed, watch } from 'vue'

const props = defineProps({
    option: Object,
    state: Object,
    trackBy: String  // This is the key to use to track the state
})

// The main goal of `selectedOption` is to allow us to render its children (selectedOption.children), if any.
let selectedOption = ref({})

// Watch changes to state[props.trackBy], eg: state.category
watch(() => props.state[props.trackBy], (value) => {
    selectedOption.value = props.option.dropdownItems?.find(item => item.id === value)
}, { immediate: true })



const setChildren = () => {
    // Example that sets the children of `selectedOption` dynamically
    selectedOption.value = {
        children: [
            { type: "text", label: "Date", id: 4, trackBy: "value" },
            { type: "text", label: "Date", id: 4, trackBy: "value" }]
    }
}

</script>

<template>
    <select v-if="option.type == 'dropdown'" v-model="state[props.trackBy]">
        <option v-for="item in option.dropdownItems" :key="item.id" :value="item.id">{{ item.label }}</option>
    </select>

    <!-- Custom component (similar to `dropdown` that has children, but children are set dynamically) -->
    <div v-if="option.type == 'user-answers'">
        <div>
            <!-- Example only: instead of a button, we can render any component -->
            <button @click="setChildren">Dynamically set children</button>
        </div>
    </div>


    <!-- Render children, if any -->
    <div v-if="selectedOption?.children">
        <div v-for="child in selectedOption.children" :key="child.id">
            <!-- PS: `key` is important here otherwise Vue will try to reuse the component in the DOM, which will have the wrong state  -->
            <VSelect :option="child" :state="state" :trackBy="child.trackBy" :key="state[props.trackBy]" />
        </div>
    </div>

    
    <!-- Leaf nodes -->
    <input v-if="option.type == 'date'" v-model="state[option.trackBy]" type="date" :id="option.id">

    <input v-if="option.type == 'text'" v-model="state[option.trackBy]" type="text" :id="option.id">


</template>


<style lang="scss" scoped>

</style>