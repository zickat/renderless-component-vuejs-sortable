<script>
// import { Sortable } from '@shopify/draggable';
const Draggable = () => import('@shopify/draggable');

export default {
  name: 'OrderedList',

  props: {
    value: {
      required: true,
    },
    itemClass: {
      default: 'ordered-list-item',
    },
    handleClass: {
      default: 'ordered-list-item-handle',
    },
    containerClass: {
      default: 'ordered-list-container',
    },
  },

  provide() {
    return {
      itemClass: this.itemClass,
      handleClass: this.handleClass,
      containerClass: this.containerClass,
    };
  },

  methods: {
    move(containers, oldIndex, newIndex, oldContainerIndex, newContainerIndex) {
      if (oldContainerIndex === newContainerIndex) {
        if (oldIndex === newIndex) return containers;

        const items = [...containers[oldContainerIndex].items];

        let withoutItem = [
          ...items.slice(0, oldIndex),
          ...items.slice(oldIndex + 1, items.length),
        ];

        const withItems = [
          ...withoutItem.slice(0, newIndex),
          items[oldIndex],
          ...withoutItem.slice(newIndex, withoutItem.length),
        ];

        containers[oldContainerIndex].items = withItems;
        return containers;
      } else {
        const item = containers[oldContainerIndex].items[oldIndex];
        const oldItems = containers[oldContainerIndex].items;
        containers[oldContainerIndex].items = [
          ...oldItems.slice(0, oldIndex),
          ...oldItems.slice(oldIndex + 1, oldItems.length),
        ];

        const newItems = containers[newContainerIndex].items;
        containers[newContainerIndex].items = [
          ...newItems.slice(0, newIndex),
          item,
          ...newItems.slice(newIndex, newItems.length),
        ];

        return containers;
      }
    },

    up(item) {
      let index = this.value.indexOf(item);

      if (index > 0 && index < this.value.length) {
        this.$emit('input', this.move(this.value, index, index - 1));
      }
    },

    down(item) {
      let index = this.value.indexOf(item);

      if (index >= 0 && index < this.value.length - 1) {
        this.$emit('input', this.move(this.value, index, index + 1));
      }
    },

    getContainerIndex(container) {
      const prefix = `${this.containerClass}-`;
      const c = container.className.split(' ').find((_) => _.includes(prefix));

      if (!c) return null;

      return Number(c.slice(prefix.length));
    },
  },

  render() {
    return this.$scopedSlots.default({
      items: this.value,
      up: this.up,
      down: this.down,
    });
  },

  mounted() {
    Draggable().then((draggable) => {
      const Sortable = draggable.Sortable;
      const sortable = new Sortable(
        this.$el.querySelectorAll(`[class^=${this.containerClass}-`),
        {
          draggable: `.${this.itemClass}`,
          handle: `.${this.handleClass}`,
          mirror: {
            constrainDimensions: true,
          },
        },
      ).on('sortable:stop', (event) => {
        const oldContainerIndex = this.getContainerIndex(
          event.data.oldContainer,
        );
        const newContainerIndex = this.getContainerIndex(
          event.data.newContainer,
        );

        if (!oldContainerIndex || !newContainerIndex) return;

        const oldIndex = event.data.oldIndex;
        const newIndex = event.data.newIndex;

        const newValues = this.move(
          this.value,
          oldIndex,
          newIndex,
          oldContainerIndex,
          newContainerIndex,
        );
        this.$emit('input', newValues);
      });

      this.$on('hook:destroyed', () => {
        sortable.destroy();
      });
    });
  },
};
</script>
