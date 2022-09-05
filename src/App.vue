<script setup>
import VSelect from "./components/VSelect.vue";
const options = [
  {
    label: "Created at", id: 1,
    options: [
      { label: "Is equals", id: 1, children: [{ type: 'date', label: "Date", id: 4 }] },
      { label: "Is empty", id: 2 },
      {
        label: "Is between", id: 3, children: [
          { type: 'date', label: "From", id: 'from' }, { type: 'date', label: "Date", id: 'to' }]
      },
    ]
  },
  {
    label: "Active", id: 2,
    options: [{ label: "Is true", id: 3 }, { label: "Is false", id: 4 }]
  }
];
</script>

<template>
  <main>
    <div class="expression-row">
      <VSelect :options="options" />
    </div>
  </main>
</template>

<style scoped>
.expression-row {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  margin: 1rem;
}

header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
