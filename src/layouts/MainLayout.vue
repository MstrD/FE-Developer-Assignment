<template>
  <q-layout view="hHh lpR fFf">
    <q-header elevated>
      <q-toolbar>

        <q-toolbar-title>
          <q-img 
            id="logo" 
            class="q-ml-sm" 
            src="~assets/pipedrive_logo.svg"
            width="100px" 
            max-height='40px' 
            :fit='scale-down' 
            position="left"
          />
        </q-toolbar-title>

      </q-toolbar>
    </q-header>

    <q-page-container>
      <div class="first-header flex">
        <div class='q-pl-lg q-pt-md q-pb-md col main-title'>People's List</div>
        <q-input class="col-12 q-mr-lg search" v-model="search" filled rounded type="search" label="Filter by name">
          <template v-slot:prepend>
            <q-icon name="search" />
          </template>
        </q-input>
      </div>
      <q-separator />

      <q-list class="q-pt-md q-pb-md">
        <Person v-for="person in persons" :key="person.name" v-bind="person" class="q-my-sm" clickable v-ripple />
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
import Person from 'src/components/Person.vue'

const linksList = [
  {
    name: 'Docs',
    company: 'quasar.dev',
  },
  {
    name: 'Github',
    company: 'github.com/quasarframework',
  },
  {
    name: 'Discord Chat Channel',
    company: 'chat.quasar.dev',
  },
  {
    name: 'Forum',
    company: 'forum.quasar.dev',
  },
  {
    name: 'Twitter',
    company: '@quasarframework',
  },
  {
    name: 'Facebook',
    company: '@QuasarFramework',
  },
  {
    name: 'Quasar Awesome',
    company: 'Community Quasar projects',
  }
];

export default ({
  name: 'MainLayout',

  components: {
    Person
  },

  data () {
    return {
      persons: linksList
    }
  }
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
</style>
