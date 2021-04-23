<template>
  <div id="app">
    <OrderedList v-model="list">
      <div>
        <div
          v-for="(container, index) in list"
          :key="container.name"
          class="container"
        >
            <h1>{{ container.name }}</h1>
            <OrderedListContainer :index="index">
              <OrderedListItem
                v-for="item in container.items"
                :key="item.id"
                class="item"
              >
                <OrderedListDraggable>
                  {{ item.name }}
                </OrderedListDraggable>
              </OrderedListItem>
          </OrderedListContainer>
        </div>
      </div>
    </OrderedList>
    {{ list }}
  </div>
</template>

<script>
import OrderedList from './lib/ordered-list/OrderedList';
import OrderedListDraggable from './lib/ordered-list/OrderedListDraggable';
import OrderedListItem from './lib/ordered-list/OrderedListItem';
import OrderedListContainer from './lib/ordered-list/OrderedListContainer';
export default {
  name: 'App',
  components: {
    OrderedListContainer,
    OrderedListItem,
    OrderedListDraggable,
    OrderedList,
  },
  data() {
    return {
      list: [
        {
          name: 'container 1',
          items: [
            { id: '1', name: 'item 1' },
            { id: '2', name: 'item 2' },
            { id: '3', name: 'item 3' },
          ],
        },
        {
          name: 'container 2',
          items: [
            { id: '4', name: 'item 4' },
            { id: '5', name: 'item 5' },
            { id: '6', name: 'item 6' },
          ],
        },
      ],
    };
  },
};
</script>

<style lang="stylus">
.item, .container {
  padding: 5px;
  border: 1px solid black;
  margin 5px
}
</style>
