<template>
    <q-dialog>
      <q-card>
        <q-card-section style="display: flex">
          <div class="text-h6"><b>Person Information</b></div>
          <q-space />
          <q-btn flat no-caps icon="close" color="primary" style="width: 10%" v-close-popup />
        </q-card-section>

        <q-separator />

        <q-card-section style="max-height: 50vh; width: 400px" class="scroll modal-header">
          <q-avatar size="100px" color="light-blue-1" text-color="blue-14" font-size="medium" />
          <div class="text-h6"><b>{{ name }} lmao</b></div>
          <div class="text-green"><b>{{ phone }} lmao 2</b></div>
        </q-card-section>

        <q-separator />

        <q-card-section style="max-height: 50vh" class="scroll">
          <div id="details">
            <div class="row">
              <div class="col-4 col-left text-bold">Email</div>
              <div class="col col-right text-light">{{ email }} lmao 2</div>
            </div>
            <div class="row">
              <div class="col-4 col-left text-bold">Organization</div>
              <div class="col col-right text-light">{{ company }} lmao 2</div>
            </div>
            <div class="row">
              <div class="col-4 col-left text-bold">Assistant</div>
              <div class="col col-right text-light">{{ assistant }} lmao 2</div>
            </div>
            <div class="row">
              <div class="col-4 col-left text-bold">Groups</div>
              <div class="col col-right text-light">{{ groups }} lmao 2</div>
            </div>
            <div class="row">
              <div class="col-4 col-left text-bold">Location</div>
              <div class="col col-right text-light">{{ location }} lmao 2</div>
            </div>
          </div>
        </q-card-section>

        <q-separator />

        <q-card-actions align="right" class="modal-footer">
          <q-btn flat no-caps class="btn" label="Back" color="primary" v-close-popup />
        </q-card-actions>
      </q-card>
      <q-item-section
      v-if="name"
      avatar
    >
    </q-item-section>
    </q-dialog>
</template>

<script>
export default ({
  name: 'PersonDetails',
  props: {
    name: {
      type: String,
      required: true
    },

    company: {
      type: String,
      default: ''
    },

    phone: {
        type: String,
        default: ''
    },

    email: {
      type: String,
      default: '',
    },

    assistant: {
      type: String,
      default: ''
    },

    groups: {
      type: String,
      default: ''
    },

    location: {
      type: String,
      default: ''
    },
  }
})
</script>

<style scoped>
  .row {
    display: flex;
    margin-bottom: 10px;
  }
  .col-left {
    text-align: right;
    padding-right: 10px;
  }
  .col-right {
    text-align: left;
    padding-left: 10px;
  }
  .modal-header {
    display: grid;
    justify-content: center;
    text-align: center;
  }
  .modal-footer {
    background-color: #EBEBEB;
    margin-bottom: 0px;
  }
  .btn {
    border: 1px solid gray;
    background-color: white;
    font-weight: bold;
  }
</style>