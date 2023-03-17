<script setup>
/* eslint-disable */
import { ref, onMounted, computed, watch } from "vue";

//подключаем ссылки к нужным элементам кода
const todos = ref([]);
const name = ref("");

const input_content = ref("");
const input_category = ref(null);

//асинхронный вывод списка
const todos_asc = computed(() =>
  todos.value.sort((a, b) => {
    return a.createdAt - b.createdAt;
  })
);

// форма ввода дела
const addTodo = () => {
  if (input_content.value.trim() === "" || input_category.value === null) {
    return;
  }

  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime(),
  });

  input_content.value = "";
  input_category.value = null;
};

//удаление дела
const removeTodo = (todo) =>
  (todos.value = todos.value.filter((t) => t !== todo));

//сохраняем todo
watch(
  todos,
  (newVal) => {
    localStorage.setItem("todos", JSON.stringify(newVal));
  },
  { deep: true }
);

//сохраняем имя
watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

//вызов из локального хранилища
onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});
</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        Привет,
        <input type="text" placeholder="введи имя здесь" v-model="name" />
      </h2>
    </section>
    <section class="create-todo">
      <h3>Создадим список дел на сегодня</h3>
      <form @submit.prevent="addTodo">
        <input
          type="text"
          placeholder="запиши свое дело здесь"
          v-model="input_content"
        />
        <h4>Выбери категорию</h4>
        <div class="options">
          <label>
            <input
              type="radio"
              name="category"
              value="business"
              v-model="input_category"
            />
            <span class="bubble business"></span>
            <div>Бизнес</div>
          </label>
          <label>
            <input
              type="radio"
              name="category"
              value="personal"
              v-model="input_category"
            />
            <span class="bubble personal"></span>
            <div>Личное</div>
          </label>
        </div>
        <input type="submit" value="Добавить дело" />
      </form>
    </section>
    <section class="todo-list">
      <h3>Список дел</h3>
      <div class="list">
        <div
          v-for="todo in todos_asc"
          :class="`todo-item ${todo.done && 'done'}`"
        >
          <label>
            <input type="checkbox" v-model="todo.done" />
            <span :class="`bubble ${todo.category}`"></span>
          </label>
          <div class="todo-content">
            <input type="text" v-model="todo.content" />
          </div>
          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Delete</button>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>
