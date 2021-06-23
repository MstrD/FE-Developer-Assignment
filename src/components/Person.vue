<template>
  <q-item
    clickable
    @click="isClicked = !isClicked"
    class="q-ml-lg q-mr-lg q-pl-lg q-pr-lg entry"
  >

    <q-item-section>
      <q-item-label>{{ name }}</q-item-label>
      <q-item-label caption>
        <q-icon name="business" />
        {{ company }}
      </q-item-label>
    </q-item-section>
    <q-item-section
      v-if="name"
      avatar
    >
      <q-avatar color="light-blue-1" text-color="blue-14" font-size="medium" />
    </q-item-section>
    <PersonDetails v-model="isClicked" />
  </q-item>
</template>

<script>
import PersonDetails from 'src/components/PersonDetails.vue'

export default ({
  name: 'Person',
  components: {
    PersonDetails
  },
  props: {
    name: {
      type: String,
      required: true
    },

    company: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      isClicked: false
    }
  }
})
</script>

<style scoped>
  .entry {
    border: 1px solid lightgray;
  }
</style>
