<template>
  <div class="home">
    <h1>My Recipes</h1>

    <button @click="togglePopup">Add new Recipe</button>
    <div class="recipes">
      <!-- Recipes here -->
      <div
        class="card"
        v-for="recipe in $store.state.recipes"
        :key="recipe.slug"
      >
        <h2>{{ recipe.title }}</h2>
        <p>{{ recipe.description }}</p>
        <router-link :to="`/recipes/${recipe.slug}`">
          <button>View Recipe</button>
        </router-link>
      </div>
    </div>

    <div v-if="popupOpen" class="add-recipe-popup">
      <div class="popup-content">
        <h2>Add new Recipe</h2>

        <form @submit.prevent="addNewRecipe">
          <div class="group">
            <label for="">Title</label>
            <input type="text" name="" id="" v-model="newRecipe.title" />
          </div>
          <div class="group">
            <label for="">Description</label>
            <textarea
              name=""
              id=""
              cols="30"
              rows="10"
              v-model="newRecipe.description"
            ></textarea>
          </div>
          <div class="group">
            <label for="">Ingredients</label>
            <div
              class="ingredients"
              v-for="i in newRecipe.ingredientRows"
              :key="i"
            >
              <input type="text" v-model="newRecipe.ingredients[i - 1]" />
            </div>
            <button @click="addNewIngredient">Add ingredients</button>
          </div>
          <div class="group">
            <label for="">Method</label>
            <div class="method" v-for="i in newRecipe.methodRows" :key="i">
              <textarea
                name=""
                id=""
                cols="30"
                rows="10"
                v-model="newRecipe.method[i - 1]"
              ></textarea>
            </div>
            <button @click="addNewStep">Add step</button>
          </div>
          <button @click="togglePopup">Close</button>
          <button type="submit" @submit.prevent="addNewRecipe">
            Add Recipe
          </button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import { mapMutations } from "vuex";
export default {
  name: "HomeView",
  data() {
    return {
      newRecipe: {
        title: "",
        description: "",
        ingredients: [],
        method: [],
        ingredientRows: 1,
        methodRows: 1,
      },
      popupOpen: false,
    };
  },
  computed: {},
  methods: {
    togglePopup: function () {
      this.popupOpen = !this.popupOpen;
    },
    addNewIngredient: function () {
      this.newRecipe.ingredientRows++;
    },
    addNewStep: function () {
      this.newRecipe.methodRows++;
    },
    addNewRecipe: function () {
      this.newRecipe.slug = this.newRecipe.title
        .toLowerCase()
        .replace(/\s/g, "-");
      if (!this.newRecipe.slug) {
        alert("Please enter a title");
        return;
      }
      console.log("this.newRecipe :>> ", this.newRecipe);
      // this.ADD_RECIPE(this.$store.state, this.newRecipe);
      console.log("this.$store.state.recipes :>> ", this.$store.state.recipes);
    },
    ...mapMutations(["ADD_RECIPE"]),
  },
};
</script>

<style>
.home {
  padding: 1rem;
  display: flex;
  flex-direction: column;
  align-items: center;
}
h1 {
  font-size: 3rem;
  margin-bottom: 32px;
}
.recipes {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
}
.recipes .card {
  padding: 1rem;
  border-radius: 5px;
  margin: 1rem;
  background-color: #081c33;
}
.recipes .card h2 {
  font-size: 2rem;
  margin-bottom: 1rem;
}
.recipes .card p {
  font-size: 1.125rem;
  line-height: 1.4;
  margin-bottom: 1rem;
}
.add-recipe-popup {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}
.add-recipe-popup .popup-content {
  background-color: #081c33;
  padding: 2rem;
  border-radius: 1rem;
  width: 100%;
  max-width: 768px;
}
.popup-content h2 {
  font-size: 2rem;
  margin-bottom: 1rem;
}
.popup-content .group {
  margin-bottom: 1rem;
}
.popup-content .group label {
  display: block;
  margin-bottom: 0.5rem;
}
.popup-content .group input,
.popup-content .group textarea {
  display: block;
  width: 100%;
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-bottom: 1rem;
}
.popup-content .group textarea {
  height: 100px;
  resize: none;
}
.popup-content button[type="submit"] {
  margin-left: 1rem;
}
</style>
