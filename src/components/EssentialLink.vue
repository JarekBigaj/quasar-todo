<template>
  <q-item
    clickable
    :to="link"
    exact
    v-ripple
    :active="isActive"
    @click="handleClick"
    active-class="my-menu-link"
  >
    <q-item-section v-if="icon" avatar>
      <q-icon :name="icon" />
    </q-item-section>

    <q-item-section>
      <q-item-label>{{ title }}</q-item-label>
      <q-item-label caption>{{ caption }}</q-item-label>
    </q-item-section>
  </q-item>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { ref } from 'vue';

export default defineComponent({
  name: 'EssentialLink',
  setup() {
    return { linkActive: ref('/') };
  },
  props: {
    title: {
      type: String,
      required: true,
    },

    caption: {
      type: String,
      default: '',
    },

    link: {
      type: String,
      default: '#',
    },

    icon: {
      type: String,
      default: '',
    },
    activeLink: {
      type: String,
      default: '/',
    },
    isActive: Boolean,
  },
  methods: {
    navigateToHome() {
      this.$router.push(this.link);
    },
    handleClick() {
      this.$emit('update:activeLink', this.link);
    },
  },
  emits: ['update:activeLink'],
});
</script>

<style lang="scss">
.my-menu-link {
  color: white;
  background: #f2c037;
}
</style>
