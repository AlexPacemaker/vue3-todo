<script setup>
/* eslint-disable */
import { ref, onMounted, computed, watch } from "vue";

//подключаем ссылки к нужным элементам кода
const todos = ref([]);
const name = ref("");

const input_content = ref("");
const input_categories = ref(null);

//асинхронный вывод списка
const todos_asc = computed(() =>
  todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  })
);

//сохраняем в локальное хранилище данные
watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

//вызов из локального хранилища


</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        Howdy?, <input type="text" placeholder="Name here" v-model="name" />
      </h2>
    </section>
  </main>
</template>
