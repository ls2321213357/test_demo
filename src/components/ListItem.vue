<script setup>
const emits = defineEmits(['delete', 'edit', 'save'])
const props = defineProps({
  itemInfo: {
    type: Object,
    required: true
  },
  isSave: {
    type: Boolean,
    required: true
  }
})
const deleteHandler = (item) => {
  emits('delete', item)
}
const editHandler = (item) => {
  emits('edit', item)
}
const saveHandler = () => {
  emits('save', false)
}
</script>
<template>
  <div class="list-item">
    <div class="info">{{ props.itemInfo.info }}</div>
    <div class="btn-list">
      <button @click="deleteHandler(props.itemInfo.info)">删除</button>
      <button @click="saveHandler"
              v-if="props.isSave">保存
      </button>
      <button @click="editHandler(props.itemInfo.info)" v-else>修改</button>
    </div>
  </div>

</template>

<style scoped>
.list-item {
  padding-left: 20px;
  width: 100%;
  height: 60px;
  margin-bottom: 10px;
  border: 1px solid #000;
}
</style>
