# vue-todo

## 개발환경설정

- vue-todo 프로젝트 폴더 생성후 열기(프로젝트 상위폴더에 빈칸, 특수문자 없어야함0)

- nodejs v18이상, 터미널(vscode 하단에서 위로 드래그) node -v로 확인

- 크롬익스텐션: Vue devtools 설치

- npm create vite@latest ./ 끝에 ./ 찍으면 현재폴더에 템플릿 세팅

- Select a framework: Vue, Select a variant: JavaScript

- npm i - package.json의 의존성 패키지 설치

- npm run dev -  로컬웹서버실행후 ctrl + 로컬주소클릭

- vscode익스텐션: Vue - Official 설치 - .vue파일 보기

- vscode익스텐션: Vetur 설치(css 코드힌트 안뜨는경우)

- style.css가 공통 css이므로 공통 css 복사 붙이기

---

## vue 기본구조

- body 하단에서 main.js 모듈 연결하여 시작

```html
// index.html
<body>
    <div id="app"></div>
    <script type="module" src="/src/main.js"></script>
  </body>
```

```js
// main.js
import { createApp } from 'vue';
import './style.css';
import App from './App.vue';

// createApp 함수에 App 객체를 전달하여 루트컴포넌트로 사용
createApp(App).mount('#app');
```

## App.vue

- 모듈이며 메인페이지 역할

- `<script setup>` - 자바스크립트 작성

- `<template>` - 뷰템플릿으로 html과 데이터 바인딩, 뷰 디렉티브등 사용

- `<style scoped>` - data-속성을 이용한 해시생성으로 모듈별 클래스명이 같아도 따로 적용됨

- 컴포넌트 파일명은 파스칼 케이스(첫단어 대문자)

```js
// App.vue
<script setup>
// vue3 컴포지션 api 가져오기
import { reactive, computed } from 'vue';

// reactive함수: 객체를 반응성으로 만들어주며 객체의 값이 변경되면 리렌더링하여 화면 업데이트
const data = reactive({
  newItem: '',
  items: [],
});

// computed함수: data.items가 변경될때만 함수 실행(계산결과를 캐싱하여 성능 최적화)
const totalItems = computed(() => data.items.length);

// 할일 배열에서 할일의 완료가 true인 요소의 배열 길이 리턴
const isComplete = computed(
  () => data.items.filter((item) => item.completed).length
);

// 할일추가: data.newItem은 입력필드와 연결되있으므로 빈값이 아닐경우 추가
// 추가된 데이터 마다 고유 id 추가하여 데이터 구분 가능하도록 해줌
// 할일추가후 입력필드 빈칸으로 초기화
function addItem() {
  if (data.newItem !== '') {
    data.items.push({
      id: data.items.length + 1,
      text: data.newItem,
      completed: false,
    });
    data.newItem = '';
  }
};

function deleteItem(id) {
  // id에 맞는 아이템 찾기
  const itemToDelete = data.items.find((item) => item.id === id);

  // 삭제할 아이템 위치 찾기
  const index = data.items.indexOf(itemToDelete);

  // splice(시작위치, 삭제갯수)
  data.items.splice(index, 1);
};
</script>

<template>
  <main class="app">
    <h1>Simple to-do list</h1>
    <div class="todo_count">
      <!-- 템플릿 내부에 데이터 표현시 {{ }} 사용 -->
      완료: {{ isComplete }} / 할 일: {{ totalItems }}
    </div>
    <div class="todo_add">
      <!-- v-model 디렉티브는 폼요소와 데이터를 양방향으로 연결 -->
      <!-- v-on:이벤트명으로 이벤트 연결 -->
      <input
        type="text"
        v-model="data.newItem"
        v-on:keyup.enter="addItem()"
        placeholder="할 일을 입력하세요"
        title="할 일을 입력하세요"
      />
      <button type="button" class="add_btn" v-on:click="addItem()">Add</button>
    </div>
    <ul class="todo_list">
      <!-- v-bind 디렉티브는 데이터를 단방향으로 연결하여 가져오기만함 -->
      <!-- 리스트는 고유 id를 key속성에 연결하여 리스트의 순서가 바뀌어도 구별가능하게함 -->
      <li
        v-for="(item, index) in data.items"
        v-bind:key="item.id"
        v-bind:class="{ completed: item.completed }"
      >
        <!-- 라벨클릭시 for로 연결된 체크박스가 클릭되어 true, false가 발생되며
        v-model="item.completed"에 의해 completed속성값이 true, false가됨 -->
        <input
          v-bind:id="`check${item.id}`"
          type="checkbox"
          v-model="item.completed"
        />
        <label v-bind:for="`check${item.id}`">{{ item.text }}</label>
        <button
          type="button"
          v-on:click="deleteItem(item.id)"
          class="remove_btn"
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
  flex-grow: 1;
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
```

## npm run build

- 기본설정은 서버루트가 기준이며 서버 base폴더 변경가능

```js
// vite.config.js
import { defineConfig } from 'vite';
import vue from '@vitejs/plugin-vue';

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()],
  base: '/vue-todo/',
});
```
