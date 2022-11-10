<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
    option: Object,
    state: Object,
    trackBy: { type: String, default: 'id' }
})

const localState = ref({
    "category": "created_at",
    "operator": "is_equals",
    "value": "1990-01-01",
})

const selectedOption = computed(() => {
    /**
     * This relates to the selected `option` in the Select component.
     * Find if any of the `options` is selected according to the `state`
     * */

    // During recursion:
    // On the first iteration state.field should be used to find the selected option
    // On the second iteration: state.operator.
    // It seems that on each iteration the `option` needs to be aware of which `field` to use
    // Let's solve that by requiring every `option` object to have a `trackBy` propertyd
    return props.option.dropdownItems?.find(option => option.id === props.state[props.trackBy])
})

</script>

<template>
    <!-- -->
    <select v-if="option.type == 'dropdown'" v-model="state[option.trackBy]">
        <option v-for="item in option.dropdownItems" :key="item.id" :value="item.id">{{ item.label }}</option>
    </select>

    <input v-if="option.type == 'date'" v-model="state[option.trackBy]" type="date" :id="option.id">

    <div v-if="option.type == 'user-answers'">
        I need to allow this component to set `selectedOption`

        <!-- Write operation failed: computed value is readonly -->
        <button @click="selectedOption = {
            children: [{type: ' date', label: 'Date', id: 4, trackBy: 'value'}],
        }">Set</button>
    </div>

    <div v-if="selectedOption?.children">
        <div v-for="child in selectedOption.children" :key="child.id">
            <VSelect :option="child" :state="state" :trackBy="child.trackBy" />

            <!-- Children, based on the selected item -->
            <!-- <div v-if="state[child.trackBy].children">
            <div v-for="(child2, idx) in state[child.trackBy].children" :key="idx">
                <input v-if="child2.type == 'date'" v-model="state[child2.trackBy]" type="date" :id="child2.id">
            </div>
        </div> -->

        </div>
    </div>

</template>


<style lang="scss" scoped>

</style>