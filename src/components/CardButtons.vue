<template>
  <div class="main-container">
    <h3 color="blue-grey-darken-3">сортировка по рейтингу</h3>
    <div class="sort-btns">
      <v-btn
        :color="active_btn === type.id ? 'blue-grey-darken-4' : 'blue-grey-darken-2'"
        size="x-small"
        v-for="type in typeSort"
        :key="type.id"
        @click="sortList(type.type, type.id)"
      >
        {{ type.name }}
      </v-btn>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const emit = defineEmits(["sortRating"]);

const active_btn = ref(2);
const typeSort = ref([
  {
    id: 0,
    name: "по возрастанию",
    type: "asc",
  },
  {
    id: 1,
    name: "по убыванию",
    type: "desc",
  },
  {
    id: 2,
    name: "без сортировки",
    type: "none",
  },
]);
const sortList = (type, id) => {
  active_btn.value = id;
  emit("sortRating", type);
};
</script>

<style>
.main-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 10px;
  width: 400px;
  border-radius: 10px;
  background-color: rgb(255, 255, 255);
}
.sort-btns {
  width: 100%;
  padding: 10px 0;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  text-align: center;
}
</style>
