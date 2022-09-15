<template>
  <div id="app">
    <select
      v-model="locale"
    >
      <option
        v-for="locale in locales"
        :key="locale.id"
        :value="locale.id"
      >
        {{ locale.name }}
      </option>
    </select>

    <Cart :locale="locale" />
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue';
import Cart from '@/components/Cart.vue'

const locale = ref('en');

const locales = computed(() => {
  return ['en', 'pl'].map(locale => {
    return {
      id: locale,
      name: locale.toUpperCase()
    }
  });
});
</script>
