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
        {{ org_name }}
      </q-item-label>
    </q-item-section>
    <q-item-section
      v-if="name"
      avatar
    >
      <q-avatar color="light-blue-1" text-color="blue-14" font-size="medium">{{ initials }}</q-avatar>
    </q-item-section>
    <PersonDetails
      v-model="isClicked"
      v-bind:id="id"
      v-bind:name="name"
      v-bind:phone="phone[0].value"
      v-bind:email="email[0].value"
      v-bind:assistant="assistant"
      v-bind:location="' ' || org_id.address"
      v-bind:organization="org_name"
      v-bind:groups="group"
      v-bind:initials="initials"
    />
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
    id: {
      type: Number,
      required: true
    },

    name: {
      type: String,
      required: true
    },

    org_name: {
      type: String,
      default: ''
    },

    org_id: {
      type: Object,
      default: ''
    },

    phone: {
      type: Object,
      default: ''
    },

    email: {
      type: Object,
      default: ''
    },

    assistant: {
      type: String,
      default: ''
    },

    group: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      isClicked: false,
      initials: this.name.replace(/[a-z]/g, '').replace(' ', '')
    }
  }
})
</script>

<style scoped>
  .entry {
    border: 1px solid lightgray;
  }
</style>
