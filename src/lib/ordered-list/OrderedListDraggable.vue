<template>
  <div :class="handleClass">
    <slot></slot>
  </div>
</template>
<script>
export default {
  name: 'OrderedListDraggable',

  inject: ['handleClass'],
};
</script>
