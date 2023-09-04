<template>
  <div class="page">
    <VirtualList :listData="data" :estimatedItemSize="100" v-slot="slotProps">
      <Item :item="slotProps.item" />
    </VirtualList>
  </div>
</template>

<script>
import VirtualList from '../../components/VirtualList.vue'
import Item from '../../components/Item.vue'

let data = []
for (let id = 0; id < 1000; id++) {
  data.push({
    id,
    value: '123', // 长文本
  })
}

export default {
  data() {
    return {
      data,
    }
  },
  components: {
    VirtualList,
    Item,
  },
  watch: {},
  props: {},
  mounted() {
    this.updateVisibleData()
    this.$refs.list.addEventListener('scroll', this.updateVisibleData)
  },
  components: {},
  methods: {
    handleToggleExpand(item) {
      item.expanded = !item.expanded
      this.updateVisibleData()
    },
    updateVisibleData() {
      const scrollTop = this.$refs.list.scrollTop
      const visibleStartIndex = Math.floor(scrollTop / ITEM_HEIGHT)
      const visibleEndIndex = visibleStartIndex + VISIBLE_ITEM_COUNT
      const visibleData = data
        .slice(visibleStartIndex, visibleEndIndex)
        .map((item, index) => {
          const top = (visibleStartIndex + index) * ITEM_HEIGHT
          const height = item.expanded
            ? ITEM_HEIGHT + getContentHeight(item)
            : ITEM_HEIGHT
          return {
            ...item,
            top,
            height,
          }
        })
      debugger
      this.visibleData = visibleData
    },
  },
}
</script>

<style lang="scss" scoped>
.page {
  width: 100%;
  // height: 100vh;
  flex-shrink: 0;
  .list-container {
    height: 500px;
    overflow-y: auto;
    position: relative;
  }

  .list {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
  }

  .list-item {
    position: absolute;
    left: 0;
    right: 0;
    font-size: 16px;
    line-height: 50px;
    border-bottom: 1px solid #ccc;
  }

  .item-header {
    padding: 0 20px;
    cursor: pointer;
  }

  .item-content {
    padding: 0 20px;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
  }
}
</style>
