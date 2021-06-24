<template>
    <q-dialog>
      <q-card>
        <q-form
          @submit="onSubmit"
        >
          <q-card-section style="display: flex; background-color: #EBEBEB">
            <div class="text-h6"><b>Add Person</b></div>
            <q-space />
            <q-btn flat no-caps icon="close" color="primary" style="width: 10%" v-close-popup />
          </q-card-section>

          <q-separator />

          <q-card-section style="max-height: 50vh; width: 400px;" class="scroll modal-header">
            <div id="avatar" class="q-mb-md">
              <q-avatar size="100px" class="row avatar" color="light-blue-1" text-color="blue-14" font-size="xxx-large">{{ initials }}</q-avatar>
            </div>
            <div id="details_main">
              <div class="row">
                <div class="col-4 col-left text-bold">Name</div>
                <q-input
                  class="col col-right text-light"
                  lazy-rules
                  :rules="[ val => val && val.length > 0 || 'Please insert a valid name']"
                  dense
                  v-model="name" />
              </div>
              <div class="row">
                <div class="col-4 col-left text-bold">Phone</div>
                <q-input class="col col-right text-light" dense v-model="phone" />
              </div>
            </div>
          </q-card-section>

          <q-separator />

          <q-card-section style="max-height: 50vh" class="scroll">
            <div id="details">
              <div class="row">
                <div class="col-4 col-left text-bold">Email</div>
                <q-input class="col col-right text-light" dense v-model="email" />
              </div>
              <div class="row">
                <div class="col-4 col-left text-bold">Organization</div>
                <q-input class="col col-right text-light" dense v-model="organization" />
              </div>
              <div class="row">
                <div class="col-4 col-left text-bold">Assistant</div>
                <q-input class="col col-right text-light" dense v-model="assistant" />
              </div>
              <div class="row">
                <div class="col-4 col-left text-bold">Groups</div>
                <q-input class="col col-right text-light" dense v-model="groups" />
              </div>
              <div class="row">
                <div class="col-4 col-left text-bold">Location</div>
                <q-input class="col col-right text-light" dense v-model="location" />
              </div>
            </div>
          </q-card-section>

          <q-separator />

          <q-card-actions align="right" class="modal-footer">
            <q-btn flat no-caps class="btn" label="Add Person" color="positive" type="submit" v-close-popup />
          </q-card-actions>
        </q-form>
      </q-card>
    </q-dialog>
</template>

<script>
import { api } from 'boot/axios';

export default ({
  name: 'NewPerson',
  data() {
    return {
      name: '',
      phone: '',
      organization: '',
      email: '',
      assistant: '',
      groups: '',
      location: '',
      initials: ''
    }
  },
  methods: {
    async getOrganization(headers) {
      await api.get('/organizations/search?api_token=994ffda10b43ea64cec09ba07cdc6ff108909d4b', {
        params: {
          term: this.organization
        }
      })
      .then((response) => {
        if (response.data.data.items.length) {
          let oldest = response.data.data.items[0].id;
          response.data.data.items.forEach(element => {
            if (element.id < oldest)
              oldest = element.id;
          });
          this.postPerson(headers, oldest);
        }
        else
          this.postOrganization(headers);
      }).catch((err) => {
        console.log(err);
      });
    },
    async postOrganization(headers) {
      await api.post('/organizations?api_token=994ffda10b43ea64cec09ba07cdc6ff108909d4b', {
          name: this.organization,
          address: this.location
        }, {
          headers: headers
      })
      .then((response) => {
        this.postPerson(headers, response.data.data.id);
      })
      .catch((error) => {
        console.log(error);
      });
    },
    async postPerson(headers, id) {
      await api.post('/persons?api_token=994ffda10b43ea64cec09ba07cdc6ff108909d4b', {
        name: this.name,
        phone: [this.phone],
        org_id: id,
        email: [this.email],
        '4fb82d351028602b86afa9228b642b08581a3848': this.assistant,
        '64c0149cea62de084df7b8fd2ea26157c5223a9a': this.groups,
      }, {
        headers: headers
      })
      .then((response) => {
        console.log('POST executed successfully', response.data);
      })
      .catch((error) => {
        console.log(error);
      });
    },
    onSubmit() {
      // headers for POST requests
      const headers = {
          'Content-Type': 'application/json;charset=UTF-8',
          'Access-Control-Allow-Origin': '*',
          'Authorization': 'JWT fefege...'
      };
      // first, verify if the organization inserted already exists;
      // then, every other request is asked in Promises
      this.getOrganization(headers);
    }
  },
  watch: {
    name: function(val) {
      this.initials = val.replace(/[^A-Z]/g, '').replace(' ', '');
    }
  }
})
</script>

<style scoped>
  .row {
    display: flex;
    margin-bottom: 10px;
  }
  .avatar {
    justify-self: center;
  }
  .col-left {
    text-align: right;
    padding-right: 10px;
  }
  .col-right {
    text-align: left;
    padding-left: 10px;
    margin-top: -10px;
  }
  #avatar {
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