<template>
  <div class="savedrecipe">
    <h2>Hier findest du deine gespeicherten Rezepte</h2>

    <div class="previews">
      <div class="recipe-teaser" v-for="(recipe, index) in myRecipes" :key="'MyRecipes+'+index">
        <div class="recipe-teaser-img">
          <img src="https://via.placeholder.com/1920x1080.png" alt="Rezept2" >
        </div>
        <div class="recipe-teaser-desc">
          <h3>{{recipe.title}}</h3>
          <p>{{recipe.description}}</p>
          <p><strong>Geschrieben von: </strong><a href="#" target="_blank" v-for="(author, index) in recipe.author" :key="index">{{author}}</a></p>
          <p><strong>Erstellt am: </strong>{{recipe.datePublished}}</p>
        </div>
        <div class="recipe-teaser-buttons">
          <router-link :to="{name: 'Recipe', params: { showRecipe: recipe }}" tag="button" class="primary-button">
            <i><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 416 512"><!-- Font Awesome Free 5.15.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free (Icons: CC BY 4.0, Fonts: SIL OFL 1.1, Code: MIT License) --><path d="M207.9 15.2c.8 4.7 16.1 94.5 16.1 128.8 0 52.3-27.8 89.6-68.9 104.6L168 486.7c.7 13.7-10.2 25.3-24 25.3H80c-13.7 0-24.7-11.5-24-25.3l12.9-238.1C27.7 233.6 0 196.2 0 144 0 109.6 15.3 19.9 16.1 15.2 19.3-5.1 61.4-5.4 64 16.3v141.2c1.3 3.4 15.1 3.2 16 0 1.4-25.3 7.9-139.2 8-141.8 3.3-20.8 44.7-20.8 47.9 0 .2 2.7 6.6 116.5 8 141.8.9 3.2 14.8 3.4 16 0V16.3c2.6-21.6 44.8-21.4 48-1.1zm119.2 285.7l-15 185.1c-1.2 14 9.9 26 23.9 26h56c13.3 0 24-10.7 24-24V24c0-13.2-10.7-24-24-24-82.5 0-221.4 178.5-64.9 300.9z"/></svg></i>Jetzt kochen
          </router-link> 
          <button type="button" class="secondary-button" @click="removeRecipeFromMyRecipes(index)"><i><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512"><!-- Font Awesome Free 5.15.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free (Icons: CC BY 4.0, Fonts: SIL OFL 1.1, Code: MIT License) --><path d="M432 32H312l-9.4-18.7A24 24 0 0 0 281.1 0H166.8a23.72 23.72 0 0 0-21.4 13.3L136 32H16A16 16 0 0 0 0 48v32a16 16 0 0 0 16 16h416a16 16 0 0 0 16-16V48a16 16 0 0 0-16-16zM53.2 467a48 48 0 0 0 47.9 45h245.8a48 48 0 0 0 47.9-45L416 128H32z"/></svg></i>Verwerfen</button>
        </div>
      </div>
      <hr>
    </div>
    </div>
</template>
<script>
export default {
  computed: {
    myRecipes() {
      return this.$store.getters.myRecipes;
    },
  },
  methods: {
    removeRecipeFromMyRecipes(index) {
      this.$store.dispatch('removeFromMyRecipes', index);
    },
  }
}
</script>
<style>
  
</style>