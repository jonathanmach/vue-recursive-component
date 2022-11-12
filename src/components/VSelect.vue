<script setup>
import { ref, computed, watch } from 'vue'

const props = defineProps({
    option: Object,
    state: Object,
    trackBy: { type: String, default: 'id' }
})
let selectedOption = ref({})

// watch state[props.trackBy]
watch(() => props.state[props.trackBy], (value) => {
    selectedOption.value = props.option.dropdownItems?.find(item => item.id === value)
    console.log(selectedOption.value);
}, { immediate: true })



const setChildren = () => {
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

    <!-- Recursion -->
    <div v-if="selectedOption?.children">
        <div v-for="child in selectedOption.children" :key="child.id">
            <!-- PS: `key` is important here otherwise Vue will try to reuse the component in the DOM, which will have the wrong state  -->
            <VSelect :option="child" :state="state" :trackBy="child.trackBy" :key="state[props.trackBy]" />
        </div>
    </div>

    <!-- Leaf nodes -->
    <input v-if="option.type == 'date'" v-model="state[option.trackBy]" type="date" :id="option.id">

    <input v-if="option.type == 'text'" v-model="state[option.trackBy]" type="text" :id="option.id">

    <div v-if="option.type == 'user-answers'">
        <div>
            <button @click="setChildren">Dynamically set children</button>
        </div>

    </div>

</template>


<style lang="scss" scoped>

</style>