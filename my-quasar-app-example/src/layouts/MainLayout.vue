<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title>
          Magasine Ipanova
        </q-toolbar-title>

        <q-img
        :src="url"
        alt="logo ipanova"
        style="height: 30px; max-width: 150px"/>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
    >
      <q-list>
        <q-item-label
          header
        >
          Liste d'articles
        </q-item-label>

        <ArticlesLinks
          v-for="link in articlesLinks"
          :key="link.title"
          v-bind="link"
        />
        <button @click="loadData">press</button>
      </q-list>
    </q-drawer>
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import ArticlesLinks from 'src/components/ArticlesLinks.vue'

const linksList = [
  {
    title: 'My first article',
    caption: 'qu\'il est trop bien',
    icon: 'list',
    link: 'https://quasar.dev'
  }
];

import { defineComponent, ref } from 'vue'
import { api } from 'boot/axios'
import { useQuasar } from 'quasar'

export default defineComponent({
  name: 'MainLayout',

  components: {
    ArticlesLinks
  },

  setup () {
    //
    const url = ref('logo-ipanova.svg')
    //
    // axios
    const $q = useQuasar()
    const data = ref(null)

    function loadData () {
      api.get('http://localhost:8055/items/articles')
        .then((response) => {
          data.value = response.data
          console.log(data.value)
          $q.notify({
            color: 'positive',
            position: 'top',
            message: 'data received',
            icon: 'report_problem'
          })
        })
        .catch(() => {
          $q.notify({
            color: 'negative',
            position: 'top',
            message: 'Loading failed',
            icon: 'report_problem'
          })
        })
    }
    //

    const leftDrawerOpen = ref(false)


    return {
      url: url,
      articlesLinks: linksList,
      leftDrawerOpen,
      data,
      loadData,
      toggleLeftDrawer () {
        leftDrawerOpen.value = !leftDrawerOpen.value
      }
    }
  }
})
</script>
