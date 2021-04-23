<template>
  <div :class="[itemClass, isDraggable ? handleClass : '']">
    <slot></slot>
  </div>
</template>
<script>
export default {
  name: 'OrderedListItem',

  inject: ['itemClass', 'handleClass'],

  props: {
    isDraggable: {
      type: Boolean,
      default: true,
    },
  },
};
</script>
<style lang="stylus">
div {
  outline: 0;
}

.draggable-mirror {
  opacity: 0.8;

  .list-item-number:before {
    color: $purple;
  }
}
</style>
