<script setup>
import {ref, reactive} from 'vue'
import ListItem from "./ListItem.vue";

const currentInput = ref('')
const editItem = ref('')
const inputHandler = (e) => {
  currentInput.value = e.target.value
}
const clearInput = () => {
  document.querySelector("input").value = "";
  currentInput.value = ''
}
const addItem = () => {
  if (state.todoList.find(item => item.info === currentInput.value)) {
    clearInput()
    alert('此条已经添加过了')
    return
  }
  if (editItem.value) {
    //修改时点击新建的话 就不进行修改操作了
    let result = state.todoList.find(item => item.info === editItem.value)
    result.isSave = false
  }
  state.todoList.push({
    info: currentInput.value,
    isSave: false
  })
  clearInput()
}
const state = reactive({
  todoList: []
})
const deleteHandler = (val) => {
  state.todoList = state.todoList.filter(item => item.info !== val)
}
const editHandler = (val) => {
  editItem.value = val
  document.querySelector("input").value = val;
  let result = state.todoList.find(item => item.info === val)
  result.isSave = true
}
const saveHandler = (val) => {
  let result = state.todoList.find(item => item.info === editItem.value)
  result.info = currentInput.value
  result.isSave = val
  clearInput()
}
</script>

<template>
  <div class="container">
    <div class="input-box">
      <input @input="inputHandler" @keyup.enter="addItem">
      <button @click="addItem">新建</button>
    </div>
    <div class="content-box">
      <ListItem v-for="item in state.todoList" :key="item"
                :itemInfo="item" @delete="deleteHandler"
                :isSave="item.isSave" @save="saveHandler"
                @edit="editHandler"
      ></ListItem>
    </div>
  </div>
</template>
<style scoped>
.container {
  width: 100%;
  height: 100%;
}

.container .input-box {
  width: 100%;
  height: 60px;
  border: 1px solid #000;
  display: flex;
  align-items: center;
  justify-content: start;
  padding: 20px;
}

.container .input-box button {
  margin-left: 10px;
}

.container .content-box {
  width: 100%;
  height: calc(100% - 60px);
  padding: 10px;
  border: 1px solid #000;
  display: flex;
  align-items: center;
  justify-content: start;
  flex-direction: column;
}
</style>
