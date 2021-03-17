<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated class="bg-black">
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="leftDrawerOpen = !leftDrawerOpen"
        />
        <q-toolbar-title>
          Global Gallery
          <q-avatar text-color="white" icon="public" />
        </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-drawer
        v-model="leftDrawerOpen"
        show-if-above
        bordered
      >
        <q-scroll-area class="fit">
          <q-list padding class="menu-list">
            <q-item-label
              header
              class="text-grey-8"
            >
              Menu
            </q-item-label>
            <EssentialLink
              v-for="link in essentialLinks"
              :key="link.title"
              v-bind="link"
            />
          </q-list>
        </q-scroll-area>
      </q-drawer>


    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import EssentialLink from 'components/EssentialLink.vue'

const linksData = [
  {
    title: 'Ver todas las fotos',
    caption: 'Ver las fotos compartidas por los demás',
    icon: 'public',
    link: '/all'
  },
  {
    title: 'Guardar una foto',
    caption: 'Comparte una fotografía para que la vea todo el mundo',
    icon: 'library_add',
    link: '/store'
  }
];

export default {
  name: 'MainLayout',
  components: { EssentialLink },
  data () {
    return {
      leftDrawerOpen: false,
      essentialLinks: linksData
    }
  }
}
</script>
