<template>
  <div id="app">
    <select
      v-model="$i18n.locale"
    >
      <option
        v-for="locale in locales"
        :key="locale.id"
        :value="locale.id"
      >
        {{ locale.name }}
      </option>
    </select>

    <Cart />
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';
import Cart from '@/components/Cart.vue'

const locales = computed(() => {
  return ['en', 'pl'].map(locale => {
    return {
      id: locale,
      name: locale.toUpperCase()
    }
  });
});
</script>
