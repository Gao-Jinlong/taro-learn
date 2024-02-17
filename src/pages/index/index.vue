<template>
  <view class="index">
    <view class="todo-list">
      <checkbox-group @change="(e) => toggleGroupChange(e.detail.value)">
        <view
          v-for="(todo, index) in todoList"
          :key="todo.id"
          class="todo-item"
        >
          <checkbox :value="todo.id" :checked="todo.done"></checkbox>
          <text :class="{ done: todo.done }" @tap="toggleTodo(todo.id)">{{
            todo.text
          }}</text>
          <button @tap="removeTodo(index)">删除</button>
        </view>
      </checkbox-group>
    </view>
    <button @tap="addTodo">添加任务</button>
  </view>
</template>
<script setup lang="ts">
import { ref } from "vue";
import { useLoad } from "@tarojs/taro";

useLoad(() => {
  console.log("生命周期 - onLoad");
});

const todoList = ref([
  { id: uniqueId(), text: "吃饭", done: true },
  { id: uniqueId(), text: "睡觉", done: false },
  { id: uniqueId(), text: "打豆豆", done: false },
]);

const addTodo = () => {
  todoList.value.push({
    id: uniqueId(),
    text: "新任务",
    done: false,
  });
};

const removeTodo = (index: number) => {
  todoList.value.splice(index, 1);
};

const toggleTodo = (id: string) => {
  console.log("toggleTodo id", id);
  const todo = todoList.value.find((todo) => todo.id === id);
  if (todo) {
    todo.done = !todo.done;
  }
};
const toggleGroupChange = (ids: string[]) => {
  console.log("toggleGroupChange ids", ids);
  todoList.value.forEach((todo) => {
    todo.done = ids.includes(String(todo.id));
  });
};

let uniqueNum = 0;
function uniqueId() {
  return (
    Math.random().toString(36).substr(2, 9) +
    (uniqueNum++).toString().padStart(4, "0")
  );
}
</script>
<style lang="scss">
.todo-list {
  .todo-item {
    display: flex;
    justify-content: space-between;
    margin-bottom: 10px;
    .done {
      text-decoration: line-through;
    }
  }
}
</style>
