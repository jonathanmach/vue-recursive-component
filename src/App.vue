<script setup>
import VSelect from "./components/VSelect.vue";
import { ref } from 'vue'

const state = ref({
  "category": "created_at",
  "operator": "is_equals",
  "value": "1990-01-01",
})

const options = {
  type: "dropdown",
  trackBy: "category",
  dropdownItems: [
    {
      label: "Creation date", id: "created_at",
      children: [
        {
          type: "dropdown", label: "Operator", id: "operator", trackBy: "operator",
          dropdownItems: [
            {
              label: "Is equals", id: "is_equals",
              children: [{ type: "date", label: "Date", id: 4, trackBy: "value" }],
            },
            { label: "Is empty", id: "is_empty" },
            {
              label: "Is between", id: "is_between", children: [
                { type: "date", label: "From", id: "from", trackBy: "from" },
                { type: "date", label: "Date", id: "to", trackBy: "to" },
              ],
            },
          ],
        },
      ],
    },
    {
      label: "Status", id: "active",
      children: [
        {
          type: "dropdown", label: "Operator", id: "operator", trackBy: "operator",
          dropdownItems: [
            { label: "Is active", id: "is_true", },
            { label: "Is not active", id: "is_false", }
          ],
        }
      ]
    },
    {
      label: "Answers", id: "user_answers",
      children: [
        {
          type: "user-answers", id: "operator", trackBy: "operator",
          children: [] // This will be built dynamically, based on the selected option
        }
      ]
    },
  ],
};
</script>

<template>
  <main>
    <div class="expression-row">
      <VSelect :option="options" :state="state" trackBy="category" />
    </div>
    <pre>{{ state }}</pre>
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
