<template>
  <q-layout view="hHh lpR fFf">
    <q-header elevated>
      <q-toolbar>

        <q-toolbar-title>
          <a href="http://www.pipedrive.com" target="_blank">
            <q-img 
              id="logo"
              alt="Pipedrive Logo"
              title="Pipedrive"
              class="q-ml-sm" 
              src="~assets/pipedrive_logo.svg"
              width="100px" 
              max-height='40px' 
              position="left"
            />
          </a>
        </q-toolbar-title>

      </q-toolbar>
    </q-header>

    <q-page-container>
      <div class="first-header flex">
        <div class='q-pl-lg q-pt-md q-pb-md col main-title'>People's List</div>
        <div class="col-8 flex">
          <q-btn dense class="col-12 q-mt-sm q-mb-md q-mr-lg" icon="person_add" @click="isAdding = !isAdding">
            <q-tooltip transition-show="flip-right" transition-hide="flip-left">Add a person</q-tooltip>
          </q-btn>
          <q-space />
          <q-input
            class="col-12 q-mr-lg q-mt-sm q-mb-sm search"
            v-model="search"
            dense
            filled
            square
            label="Filter by name..."
            type="text"
          >
            <template v-slot:prepend>
              <q-icon name="search" />
            </template>
          </q-input>
        </div>
      </div>

      <NewPerson v-model="isAdding" />

      <q-list class="q-pt-md q-pb-md">
        <draggable :list="persons" ghost-class="ghost-card" draggable=".item" :animation="500" @start="drag=true" @end="drag=false">
          <transition-group>
          <Person v-for="person in persons" :key="person.id" v-bind="person" class="q-my-sm item" clickable v-ripple />
          </transition-group>
        </draggable>
      </q-list>

      <div class="wrapper">
        <q-btn no-caps class="btn">Load more</q-btn>
      </div>

      <router-view />
    </q-page-container>
    <q-footer class="footer">
      <q-toolbar>
        <q-toolbar-title />
      </q-toolbar>
    </q-footer>
  </q-layout>
</template>

<script>
import Person from 'src/components/Person.vue';
import NewPerson from 'src/components/NewPerson.vue';

import { api } from 'boot/axios';
import { VueDraggableNext } from 'vue-draggable-next';
import { Notify } from 'quasar';

export default ({
  name: 'MainLayout',
  components: {
    Person,
    NewPerson,
    draggable: VueDraggableNext
  },
  data () {
    return {
      persons: null,
      personsCopy: null,
      isAdding: false,
      drag: false,
      search: ''
    }
  },
  mounted() {
    // API key
    api.get('/persons?api_token=994ffda10b43ea64cec09ba07cdc6ff108909d4b')
      .then((response) => {
        this.persons = Array.from(response.data.data);

        // FIX: change keys of created fields when adding persons
        this.persons.forEach(el => {
   
          let oldKeys = ["4fb82d351028602b86afa9228b642b08581a3848", "64c0149cea62de084df7b8fd2ea26157c5223a9a"],
              newKeys = ['assistant', 'group'];
          for (let i = 0; i < oldKeys.length; i++) {
            if (oldKeys[i] !== newKeys[i]) {
              Object.defineProperty(el, newKeys[i], Object.getOwnPropertyDescriptor(el, oldKeys[i]));
              delete el[oldKeys[i]];
            }
          }
        });
        // needed for search purposes
        this.personsCopy = JSON.parse(JSON.stringify(this.persons));
      })
      .catch((e) => {
        Notify.create({
          message: 'Error getting persons from API.',
          caption: 'Please, try to reload the page.',
          color: 'negative'
        });
      });
  },
  watch: {
    search: function(val) {
      val = val.toLowerCase();
      if (!val)
        this.persons = this.personsCopy;
      else {
        this.persons = [this.personsCopy.filter((el) => {
          let name = el.name.toLowerCase();
          return name.includes(val);
        })][0];
      }
    }
  },
})
</script>

<style scoped>
  #logo {
    fill: white;
    width: auto;
  }
  .main-title {
    font-weight: bold;
    font-size: 16pt;
  }
  .first-header {
    position: -webkit-sticky;
    position: sticky;
    top: 50px;
    background-color: white;
    z-index: 3;
    border-bottom: 3px solid #EBEBEB;
  }
  .search {
    font-weight: bold;
  }
  .wrapper {
    text-align: center;
  }
  .btn {
    display: inline-block;
    font-weight: bold;
    border: 1px solid #EBEBEB;
  }
  .footer {
    background-color: #EBEBEB;
  }
  .ghost-card {
    opacity: 0.5;
    background: #F7FAFC;
    border: 1px solid #4299e1;
  }
  .item:hover, .item:active, .item:focus {
    cursor: move !important;
  }
</style>
