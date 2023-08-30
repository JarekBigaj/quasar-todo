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
      </q-toolbar>
      <div class="q-px-lg q-pt-xl q-mb-md">
        <div class="text-h3">Todo</div>
        <div class="text-sutitle1">{{ todaysDate }}</div>
      </div>
      <q-img
        srcset="/src/statics/waterfall-640.jpg 640w, /src/statics/waterfall-1280.jpg 1280w, /src/statics/waterfall-1920.jpg 1920w"
        src="/src/statics/waterfall-1920.jpg"
        class="header-image absolute-top"
      />
    </q-header>

    <q-drawer
      show-if-above
      v-model="leftDrawerOpen"
      side="left"
      :width="300"
      :breakpoint="600"
      elevated
    >
      <q-img
        class="absolute-top"
        style="height: 185px"
        src="/src/statics/waterfall-640.jpg"
      >
        <div class="absolute-bottom bg-transparent">
          <q-avatar size="56px" class="q-mb-sm">
            <img src="/src/statics/AAA_0324.JPG" />
          </q-avatar>
          <div class="text-weight-bold">Jarek Bigaj</div>
        </div>
      </q-img>
      <q-list
        bordered
        padding
        class="rounded-borders text-primary"
        style="height: calc(100%-185px); margin-top: 185px"
      >
        <!-- <q-item-label header> Essential Links </q-item-label> -->

        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
          @click="handleClick(link.link)"
          :is-active="clickedLink === link.link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <keep-alive>
        <router-view />
      </keep-alive>
    </q-page-container>
  </q-layout>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import EssentialLink from 'components/EssentialLink.vue';
import { date } from 'quasar';

const linksList = [
  {
    title: 'ToDo List',
    caption: 'Tasks list',
    icon: 'list',
    link: '/',
    clicked: false,
  },
  {
    title: 'Help',
    caption: '',
    icon: 'help',
    link: '/help',
    clicked: false,
  },
];

export default defineComponent({
  name: 'MainLayout',

  components: {
    EssentialLink,
  },

  setup() {
    const leftDrawerOpen = ref(false);
    const clickedLink = ref('/');

    const handleClick = (link: string) => {
      clickedLink.value = link;
    };

    return {
      essentialLinks: linksList,
      leftDrawerOpen,
      clickedLink,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value;
      },
      handleClick,
    };
  },
  computed: {
    todaysDate() {
      const timeStamp = Date.now();
      return date.formatDate(timeStamp, 'dddd D MMMM');
    },
  },
});
</script>

<style lang="scss">
.header-image {
  height: 100%;
  z-index: -999;
  opacity: 0.4;
  filter: grayscale(100%);
}
</style>
