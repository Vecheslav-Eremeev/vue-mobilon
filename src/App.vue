<template>
  <main>
    <div class="btn-container">
      <CardButtons
        v-for="item in listForSort"
        :key="item.id"
        @sortRating="(type) => sortRating(type, item.id)"
      />
    </div>

    <div class="card-list-container">
      <CardList
        v-for="(item, index) in CardListOptions"
        :key="index"
        :options="item"
      />
    </div>
  </main>
</template>

<script setup>
import { ref, provide, watch } from "vue";
import axios from "axios";
import CardList from "./components/CardList.vue";
import CardButtons from "./components/CardButtons.vue";

const firstList = ref([]);
const secondList = ref([]);
const lastList = ref([]);

const sortTypeFirst = ref("none");
const sortTypeSecond = ref("none");
const sortTypeLast = ref("none");

const listForSort = ref([
  {
    id: 0,
    title: "firstList",
  },
  {
    id: 1,
    title: "secondList",
  },
  {
    id: 2,
    title: "lastList",
  },
]);

const sortList = (list, type) => {
  if (type === "desc") return list.sort((a, b) => b.rating.rate - a.rating.rate);
  if (type === "asc") return list.sort((a, b) => a.rating.rate - b.rating.rate);
  return list.sort((a, b) => a.id - b.id);
};

const sortRating = (type, id) => {
  sortTypeFirst.value
  switch (id) {
    case 0:
      sortTypeFirst.value = type;
      break;
    case 1:
      sortTypeSecond.value = type;
      break;
    case 2:
      sortTypeLast.value = type;
      break;
  }
};

const CardListOptions = ref([
  {
    color: "#0aa6e362",
    title: "Необработанные",
    id: 1,
  },
  {
    color: "#e86405bd",
    title: "В работе",
    id: 2,
  },
  {
    color: "#9400d364",
    title: "Завершенные",
    id: 3,
  },
]);

const getAllCards = async () => {
  try {
    await axios
      .get("https://fakestoreapi.com/products")
      .then((res) => (firstList.value = res.data));
    console.log("getAllCards - успешно");
  } catch (error) {
    console.log(error);
    alert("Упс! Ошибка получения данных ;(");
  }
};

watch(sortTypeFirst, () => {
  firstList.value = sortList(firstList.value, sortTypeFirst.value);
});

watch(sortTypeSecond, () => {
  secondList.value = sortList(secondList.value, sortTypeSecond.value);
});

watch(sortTypeLast, () => {
  lastList.value = sortList(lastList.value, sortTypeLast.value);
});

getAllCards();

provide("firstList", firstList);
provide("secondList", secondList);
provide("lastList", lastList);
</script>

<style scoped>
main {
  margin: 20px 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 100%;
}
.card-list-container {
  margin: 4px 0;
  display: flex;
  justify-content: center;
  gap: 5%;
  width: 100%;
}
.btn-container {
  display: flex;
  justify-content: center;
  gap: 5%;
  width: 100%;
}
</style>
