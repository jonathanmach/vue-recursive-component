<script setup>
import { ref } from 'vue'

const props = defineProps({
    options: Array
})

const selected = ref(null)
</script>

<template>
    <select v-model="selected">
        <option v-for="option in options" :key="option.label" :value="option">{{ option.label }}</option>
    </select>
    <VSelect v-if="selected?.options" :options="selected.options" />
    <div v-else-if="selected?.children">
        <div v-for="(child, idx) in selected.children" :key="idx">
            <input v-if="child.type == 'date'" type="date" id="start" name="trip-start" value="2018-07-22"
                min="2018-01-01" max="2018-12-31">

        </div>
    </div>

</template>


<style lang="scss" scoped>
</style>