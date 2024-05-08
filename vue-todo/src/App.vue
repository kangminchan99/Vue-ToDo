<script setup>
import { reactive, computed } from 'vue';

// data객체를 reactive함수를 통해 반응성 주입하여 초기화
const data = reactive({
  newItem: '',
  items: [],
});

function addItem() {
  if (data.newItem !== '') {
    data.items.push({
      id: data.items.length + 1,
      text: data.newItem,
      completed: false,
    });
    data.newItem = '';
  }
}

// 할일 갯수
const totalItems = computed(() => data.items.length);

function deleteItem() {
  // id가 일치하는 요소 찾기
  data.items.find((item) => item.id === id);
  data.items.splice(index, 1);
}

// 완료 갯수
const isComplete = computed(
  () => data.items.filter((item) => item.completed).length
);
</script>

<template>
  <main class="app">
    <h1>Simple to-do list</h1>
    <div class="todo_count">
      완료: {{ isComplete }} / 할 일: {{ totalItems }}
    </div>
    <div class="todo_add">
      <input
        type="text"
        placeholder="할 일을 입력하세요"
        title="할 일을 입력하세요"
        v-model="data.newItem"
        v-on:keyup.enter="addItem()"
      />
      <button type="button" class="add_btn" v-on:click="addItem()">Add</button>
    </div>
    <ul class="todo_list">
      <li
        v-for="item in data.items"
        v-bind:key="item.id"
        v-bind:class="{ completed: item.completed }"
      >
        <input
          type="checkbox"
          v-bind:id="`check${item.id}`"
          v-model="item.completed"
        />
        <label v-bind:for="`check${item.id}`">{{ item.text }}</label>
        <button
          type="button"
          class="remove_btn"
          v-on:click="deleteItem(item.id)"
        >
          Remove
        </button>
      </li>
    </ul>
  </main>
</template>

<style scoped>
.app {
  padding: 40px;
}
.app h1 {
  font-size: 30px;
  font-weight: 700;
  margin-bottom: 20px;
}

.todo_count {
  margin: 10px 0;
}

.todo_add {
  display: flex;
}
.todo_add input[type='text'] {
  height: 40px;
  padding: 0 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  width: calc(100% - 60px);
}
.todo_add .add_btn {
  height: 40px;
  padding: 0 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin-left: 10px;
  color: #fff;
  background: #333;
  border: none;
}

.todo_list {
  margin-top: 20px;
}
.todo_list li {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}
.todo_list label {
  flex-grow: 1;
}
.todo_list li.completed label {
  color: #ccc;
  text-decoration: line-through;
}
.todo_list .remove_btn {
  margin-left: auto;
  height: 32px;
  padding: 0 5px;
  background: none;
  border: 1px solid #b83030;
  color: #b83030;
  border-radius: 4px;
  margin-left: 20px;
}
</style>
