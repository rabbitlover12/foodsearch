<script setup>
import { ref } from 'vue';

const food = ref(null);

fetch('https://www.themealdb.com/api/json/v1/1/random.php')
  .then(res => res.json())
  .then(json => {
    let data = json.meals[0];
    data.ingredients = [];
    for (let i = 1; i <= 20; i++) {
      if (data[`strIngredient${i}`]) {
        let ingredient = {
          name: data[`strIngredient${i}`],
          measure: data[`strMeasure${i}`]
        };
        data.ingredients.push(ingredient);
      } else {
        break;
      }
    }
    food.value = data;
  })
  .catch(error => console.log(error));
</script>

<template>
  <div>
    <div class="bg-white text-black shadow rounded-xl">
      <img 
        :src="food.strMealThumb"
        class="rounded-t-xl"
        alt=" "
      >
      <div class="text-3xl font-bold mt-4">
        {{ food.strMeal }}
      </div>

      <div class="mt-16">
        <div 
          v-for="i in food.ingredients"
          class="grid grid-cols-2"
          :key="i.name"
        >
          <div class="text-right pr-2 font-bold">{{ i.name }}</div>
          <div class="text-left pl-2">{{ i.measure }}</div>
        </div>
      </div>

      <div class="p-8">
        <a 
          :href="food.strYoutube"
          class="bg-blue-200 text-white px-8 py-4 rounded-xl font-bold hover:text-white hover:bg-blue-500"
        >
          조리법 영상
        </a>
      </div>
    </div>
  </div>
</template>
