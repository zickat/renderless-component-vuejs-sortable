<template>
  <div :class="[`${containerClass}-${index}`]">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: 'OrderedListContainer',
  inject: ['containerClass'],

  props: {
    index: {
      type: Number,
      required: true,
    },
  },
};
</script>

<style scoped></style>
