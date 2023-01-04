<template>
  <div class="flex flex-col items-center justify-start w-full h-full">
    <div class="flex items-center justify-center mt-20">
      <div class="flex flex-col">
        <label class="mb-2">Put the ingredients in the fridge</label>
        <input type="text" class="border border-solid border-black rounded-xl py-2 px-4 w-96" v-model="ingredient"/>
        <ul class="flex mt-1.5">
          <li v-for="item in ingredients" :key="item" class="text-xs">{{ item }}{{", "}}</li>
        </ul>
        <div class="flex justify-between">
          <button @click="addIngredient" class="px-4 py-2 rounded-xl border border-solid border-black text-sm">Add Ingredient</button>
          <button @click="searchRecipes" class="ml-4 px-4 py-2 rounded-xl border border-solid border-amber-500 bg-amber-500 text-sm">Search Recipes</button>
        </div>
      </div>
    </div>
    <div class="flex flex-wrap items-center justify-center mt-14 w-full">
      <div class="m-4 py-4 px-4 h-[600px] w-72 border border-black rounded-2xl shadow relative" v-for="recipe in recipes" :key="recipe.id">
        <span class="absolute top-1 right-2 text-xs">Likes: {{recipe.likes}}</span>
        <h1 class="text-lg font-bold">{{ recipe.title }}</h1>
        <img class="w-full h-40" :src="recipe.image" :alt="recipe.title"/>
        <p class="text-start mt-2 text-sm font-bold">Ingredients you have:</p>
        <ul class="flex items-start flex-col text-xs h-14 overflow-y-scroll">
          <li v-for="ingredient in recipe.usedIngredients" :key="ingredient"> {{ ingredient.name }} - {{ingredient.amount}} {{ingredient.unit}}{{", "}} </li>
        </ul>
        <p class="text-start mt-2 text-sm font-bold">Ingredients you need:</p>
        <ul class="flex items-start flex-col text-xs h-14 overflow-y-scroll">
          <li v-for="missedIngredient in recipe.missedIngredients" :key="missedIngredient"> {{ missedIngredient.name }} - {{missedIngredient.amount}} {{missedIngredient.unit}}{{", "}} </li>
        </ul>
        <p class="text-start mt-2 text-sm font-bold">Instructions:</p>
        <ul class="flex items-start flex-col text-xs h-40 overflow-y-scroll">
          <li class="text-start" v-for="instructions in recipe.usedIngredients" :key="instructions">{{ instructions.original }}</li>
          <li class="text-start" v-for="missedInstructions in recipe.missedIngredients" :key="missedInstructions">{{ missedInstructions.original }}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import {reactive,ref} from 'vue'
export default {
  name: 'HomeView',
  components:{},
  setup() {
    const ingredients = reactive([]);
    const ingredient = ref('');
    const recipes = ref([]);

    return {
      ingredients,
      ingredient,
      recipes
    };
  },
  methods: {
    addIngredient() {
      this.ingredients.push(this.ingredient);
      this.ingredient = '';
    },

    searchRecipes() {
      const options = {
        method: 'GET',
        url: 'https://spoonacular-recipe-food-nutrition-v1.p.rapidapi.com/recipes/findByIngredients',
        params: {
          ingredients: this.ingredients.join(','),
          number: '10',
          ignorePantry: 'true',
          ranking: '1'
        },
        headers: {
          'X-RapidAPI-Key': '73843b6ab8msh7b5740722dabbbap157296jsnde3d81bb7556',
          'X-RapidAPI-Host': 'spoonacular-recipe-food-nutrition-v1.p.rapidapi.com'
        }
      };
      axios.request(options).then(response => {
        this.recipes = response.data;
        this.ingredients = '';
        console.log(this.recipes);
        console.log(response.data);
      }).catch(error => {
        console.error(error);
      });
    }
  }
}
</script>
<style>
.shadow{
  box-shadow: rgba(50, 50, 93, 0.25) 0px 50px 100px -20px, rgba(0, 0, 0, 0.3) 0px 30px 60px -30px, rgba(10, 37, 64, 0.35) 0px -2px 6px 0px inset;
}
</style>
