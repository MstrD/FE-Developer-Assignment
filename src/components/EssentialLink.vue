<template>
  <q-item
    clickable
    to=""
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
  </q-item>
</template>

<script>
export default ({
  name: 'EssentialLink',
  props: {
    name: {
      type: String,
      required: true
    },

    company: {
      type: String,
      default: ''
    }
  }
})
</script>

<style scoped>
  .entry {
    border: 1px solid lightgray;
  }
</style>
