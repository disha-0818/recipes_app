<template>
  <main class="container mt-5">
    <div class="row">
      <div class="col-12 text-right mb-4">
        <div class="d-flex justify-content-between">
          <h3>La Recipes</h3>
          <nuxt-link to="/recipes/add" class="btn btn-info">Add Recipe</nuxt-link>
        </div>
      </div>
      <template v-for="recipe in recipes">
        <div :key="recipe.id" class="col-lg-3 col-md-4 col-sm-6 mb-4">
          <recipe-card :onDelete="deleteRecipe" :recipe="recipe"></recipe-card>
        </div>
      </template>
    </div>
  </main>
</template>

<script>
console.log("We are here");
import RecipeCard from "~/components/RecipeCard.vue";

export default {
  head() {
    return {
      title: "Recipes list"
    };
  },
  components: {
    RecipeCard
  },
  async asyncData({ $axios, params }) {
    try {
      console.log("Starting the axios call");
      let recipes = await $axios.$get(`http://localhost:8000/api/recipes`);
      console.log("The recipes are: " + recipes); 
      return { recipes };
    } catch (e) {
      console.log(`error is: ${e}`)
      return { recipes: [] };
    }
  },
  data() {
    return {
    };
  },
  methods: {
    async deleteRecipe(recipe_id) {
      try {
        await this.$axios.$delete(`/recipes/${recipe_id}/`); // delete recipe
        let newRecipes = await this.$axios.$get("/recipes/"); // get new list of recipes
        this.recipes = newRecipes; // update list of recipes
      } catch (e) {
        console.log(e);
      }
    }
  }
};
</script>
<style scoped>
</style>