<template>
  <button
    :class="[
      'base-button',
      {
        'base-button--primary': variant === 'primary'
      }
    ]"
    @click="$emit('click')"
  >
    <slot />
  </button>
</template>

<script setup lang="ts">
withDefaults(defineProps<{
  variant: 'primary';
}>(), {
  variant: 'primary'
});
</script>

<style lang="scss">
.base-button {
  --button-color: transparent;
  --button-hover-color: transparent;
  --button-text-color: var(--text-color);

  margin-left: auto;
  display: inline-block;
  padding: 1.6rem;
  border-radius: 0.4rem;
  border: none;
  font-weight: bold;
  background-color: var(--button-color);
  color: var(--button-text-color);
  transition: all 0.1s linear;
  cursor: pointer;

  &:hover {
    --button-color: var(--button-hover-color);
  }

  &--primary {
    --button-color: var(--primary-color);
    --button-hover-color: var(--primary-hover-color);
    --button-text-color: var(--white);
  }
}
</style>
