<template> 
 <div class="recipe-view">
    <h2 class="recipe-title">{{showRecipe.title}}</h2>
    <div class="recipe-actions-buttons">
      <button onclick="window.print()" type="button" class="primary-button"><i><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><!-- Font Awesome Free 5.15.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free (Icons: CC BY 4.0, Fonts: SIL OFL 1.1, Code: MIT License) --><path d="M448 192V77.25c0-8.49-3.37-16.62-9.37-22.63L393.37 9.37c-6-6-14.14-9.37-22.63-9.37H96C78.33 0 64 14.33 64 32v160c-35.35 0-64 28.65-64 64v112c0 8.84 7.16 16 16 16h48v96c0 17.67 14.33 32 32 32h320c17.67 0 32-14.33 32-32v-96h48c8.84 0 16-7.16 16-16V256c0-35.35-28.65-64-64-64zm-64 256H128v-96h256v96zm0-224H128V64h192v48c0 8.84 7.16 16 16 16h48v96zm48 72c-13.25 0-24-10.75-24-24 0-13.26 10.75-24 24-24s24 10.74 24 24c0 13.25-10.75 24-24 24z"/></svg></i>Rezept drucken</button>
      <button v-if="this.showSaveButton" @click="saveFriendRecipe(showRecipe.id)" type="button" class="other-button"><i><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 384 512"><!-- Font Awesome Free 5.15.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free (Icons: CC BY 4.0, Fonts: SIL OFL 1.1, Code: MIT License) --><path d="M0 512V48C0 21.49 21.49 0 48 0h288c26.51 0 48 21.49 48 48v464L192 400 0 512z"/></svg></i>Rezept markieren</button>
      <button type="button" class="secondary-button"><i><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512"><!-- Font Awesome Free 5.15.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free (Icons: CC BY 4.0, Fonts: SIL OFL 1.1, Code: MIT License) --><path d="M448 80v352c0 26.51-21.49 48-48 48H48c-26.51 0-48-21.49-48-48V80c0-26.51 21.49-48 48-48h352c26.51 0 48 21.49 48 48zM304 296c-14.562 0-27.823 5.561-37.783 14.671l-67.958-40.775a56.339 56.339 0 0 0 0-27.793l67.958-40.775C276.177 210.439 289.438 216 304 216c30.928 0 56-25.072 56-56s-25.072-56-56-56-56 25.072-56 56c0 4.797.605 9.453 1.74 13.897l-67.958 40.775C171.823 205.561 158.562 200 144 200c-30.928 0-56 25.072-56 56s25.072 56 56 56c14.562 0 27.823-5.561 37.783-14.671l67.958 40.775a56.088 56.088 0 0 0-1.74 13.897c0 30.928 25.072 56 56 56s56-25.072 56-56C360 321.072 334.928 296 304 296z"/></svg></i>Rezept teilen</button>
    </div>
    <div class="recipe-labels">
      <label class="info-label"><i><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><!-- Font Awesome Free 5.15.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free (Icons: CC BY 4.0, Fonts: SIL OFL 1.1, Code: MIT License) --><path d="M256,8C119,8,8,119,8,256S119,504,256,504,504,393,504,256,393,8,256,8Zm92.49,313h0l-20,25a16,16,0,0,1-22.49,2.5h0l-67-49.72a40,40,0,0,1-15-31.23V112a16,16,0,0,1,16-16h32a16,16,0,0,1,16,16V256l58,42.5A16,16,0,0,1,348.49,321Z"/></svg></i> {{totalTime}}</label>
      <label class="info-label"><i><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><!-- Font Awesome Free 5.15.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free (Icons: CC BY 4.0, Fonts: SIL OFL 1.1, Code: MIT License) --><path d="M480.1 31.9c-55-55.1-164.9-34.5-227.8 28.5-49.3 49.3-55.1 110-28.8 160.4L9 413.2c-11.6 10.5-12.1 28.5-1 39.5L59.3 504c11 11 29.1 10.5 39.5-1.1l192.4-214.4c50.4 26.3 111.1 20.5 160.4-28.8 63-62.9 83.6-172.8 28.5-227.8z"/></svg></i> {{showRecipe.difficultyLevel}}</label>
      <label class="info-label"><i><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512"><!-- Font Awesome Free 5.15.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free (Icons: CC BY 4.0, Fonts: SIL OFL 1.1, Code: MIT License) --><path d="M0 464c0 26.5 21.5 48 48 48h352c26.5 0 48-21.5 48-48V192H0v272zm64-192c0-8.8 7.2-16 16-16h288c8.8 0 16 7.2 16 16v64c0 8.8-7.2 16-16 16H80c-8.8 0-16-7.2-16-16v-64zM400 64h-48V16c0-8.8-7.2-16-16-16h-32c-8.8 0-16 7.2-16 16v48H160V16c0-8.8-7.2-16-16-16h-32c-8.8 0-16 7.2-16 16v48H48C21.5 64 0 85.5 0 112v48h448v-48c0-26.5-21.5-48-48-48z"/></svg></i> {{showRecipe.datePublished}}</label>
      <label class="info-label"><i><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><!-- Font Awesome Free 5.15.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free (Icons: CC BY 4.0, Fonts: SIL OFL 1.1, Code: MIT License) --><path d="M256 288c79.5 0 144-64.5 144-144S335.5 0 256 0 112 64.5 112 144s64.5 144 144 144zm128 32h-55.1c-22.2 10.2-46.9 16-72.9 16s-50.6-5.8-72.9-16H128C57.3 320 0 377.3 0 448v16c0 26.5 21.5 48 48 48h416c26.5 0 48-21.5 48-48v-16c0-70.7-57.3-128-128-128z"/></svg></i> {{showRecipe.author.name}}</label>
    </div>

    <h2 class="recipe-title">Portionen</h2> <span>{{showRecipe.portion}} Portionen</span>
    <h2 class="recipe-title"> Zutaten</h2>
      <table class="ingredient-list">
        <tr>
          <th>Menge</th>
          <th>Name</th>
          <th>Herkunft</th>
          <th>Alternativen</th>
        </tr>
        <tr v-for="(ingredient, index) in showRecipe.ingredients" :key="'ingredient'+index" >
          <td>{{ingredient.amount}} {{ingredient.unit}}</td>
          <td>{{ingredient.name}}</td>
          <td><span v-for="(origin, index) in ingredient.origins" :key="'origin'+index">{{origin.name}}, </span></td>
          <td v-for="(altIngredient, index) in ingredient.alternativeIngredients" :key="'altIngredient'+index">
            <button class="alt-ingredient-btn" v-if="altIngredient.name.length>0">
              <i><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><!-- Font Awesome Free 5.15.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free (Icons: CC BY 4.0, Fonts: SIL OFL 1.1, Code: MIT License) --><path d="M440.65 12.57l4 82.77A247.16 247.16 0 0 0 255.83 8C134.73 8 33.91 94.92 12.29 209.82A12 12 0 0 0 24.09 224h49.05a12 12 0 0 0 11.67-9.26 175.91 175.91 0 0 1 317-56.94l-101.46-4.86a12 12 0 0 0-12.57 12v47.41a12 12 0 0 0 12 12H500a12 12 0 0 0 12-12V12a12 12 0 0 0-12-12h-47.37a12 12 0 0 0-11.98 12.57zM255.83 432a175.61 175.61 0 0 1-146-77.8l101.8 4.87a12 12 0 0 0 12.57-12v-47.4a12 12 0 0 0-12-12H12a12 12 0 0 0-12 12V500a12 12 0 0 0 12 12h47.35a12 12 0 0 0 12-12.6l-4.15-82.57A247.17 247.17 0 0 0 255.83 504c121.11 0 221.93-86.92 243.55-201.82a12 12 0 0 0-11.8-14.18h-49.05a12 12 0 0 0-11.67 9.26A175.86 175.86 0 0 1 255.83 432z"/></svg></i>
              Austauschen durch: {{altIngredient.name}}
            </button>
          </td>
        </tr>
      </table>

    <hr>

    <h2 class="recipe-title">Benötigte Haushaltsgeräte</h2>
      <span v-for="(utensil, index) in showRecipe.householdUtensils" :key="'utensil'+index" >
          {{utensil.name}},
      </span>

    <hr>

       <div class="recipe-labels">
         <label class="attention-label"><i><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><!-- Font Awesome Free 5.15.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free (Icons: CC BY 4.0, Fonts: SIL OFL 1.1, Code: MIT License) --><path d="M256,8C119,8,8,119,8,256S119,504,256,504,504,393,504,256,393,8,256,8Zm92.49,313h0l-20,25a16,16,0,0,1-22.49,2.5h0l-67-49.72a40,40,0,0,1-15-31.23V112a16,16,0,0,1,16-16h32a16,16,0,0,1,16,16V256l58,42.5A16,16,0,0,1,348.49,321Z"/></svg></i> Arbeitszeit ca. {{showRecipe.prepTime}} Minuten</label>
         <label class="attention-label"><i><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><!-- Font Awesome Free 5.15.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free (Icons: CC BY 4.0, Fonts: SIL OFL 1.1, Code: MIT License) --><path d="M256,8C119,8,8,119,8,256S119,504,256,504,504,393,504,256,393,8,256,8Zm92.49,313h0l-20,25a16,16,0,0,1-22.49,2.5h0l-67-49.72a40,40,0,0,1-15-31.23V112a16,16,0,0,1,16-16h32a16,16,0,0,1,16,16V256l58,42.5A16,16,0,0,1,348.49,321Z"/></svg></i> Koch-/Backzeit ca. {{showRecipe.cookTime}} Minuten</label>
         <label class="attention-label"><i><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><!-- Font Awesome Free 5.15.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free (Icons: CC BY 4.0, Fonts: SIL OFL 1.1, Code: MIT License) --><path d="M256,8C119,8,8,119,8,256S119,504,256,504,504,393,504,256,393,8,256,8Zm92.49,313h0l-20,25a16,16,0,0,1-22.49,2.5h0l-67-49.72a40,40,0,0,1-15-31.23V112a16,16,0,0,1,16-16h32a16,16,0,0,1,16,16V256l58,42.5A16,16,0,0,1,348.49,321Z"/></svg></i> Ruhezeit ca. {{showRecipe.restTime}} Minuten</label>
       </div>
      
       <h2 class="recipe-title"> Zubereitung</h2>
       <table class="instructions-list">
        <tr>
          <th><h2 class="recipe-title">Schritt</h2></th>
          <th><h2 class="recipe-title">Anleitung</h2></th>
          <th><h2 class="recipe-title">Geschichtlicher Hinweis</h2></th>
          <th><h2 class="recipe-title">Tipps und Tricks</h2></th>
        </tr>
        <tr v-for="(step, index) in showRecipe.recipeInstructions" :key="'step'+index" >
          <td width="10%">{{step.step}}. Schritt</td>
          <td width="60%">{{step.description}}</td>
          <td v-if="step.historicalNote[0].text.length>0">
            <span v-for="(historical,index) in step.historicalNote" :key="'historical'+index">{{historical.text}} <span style="font-weight: bold;">{{historical.author}}</span></span>
          </td>
          <td v-if="step.tipsAndTricks[0].text.length>0">
            <span v-for="(tip,index) in step.tipsAndTricks" :key="'tip'+index">{{tip.text}} <span style="font-weight: bold;">{{tip.author}}</span></span>
          </td>
        </tr>
       </table>
    <hr>
 </div>

 
</template>



<script>
  export default{
    data(){
        return{
            showSaveButton: true
        }
    },
    computed: {
      showRecipe () {
        return this.$route.params.showRecipe
      },
      totalTime () {
        var timeInMinutes = parseInt(this.showRecipe.cookTime) + parseInt(this.showRecipe.prepTime) + parseInt(this.showRecipe.restTime);
        var hours, restTime = 0;
        var output = "";
        if(timeInMinutes > 60) {
          restTime = timeInMinutes % 60;
          hours = (timeInMinutes - restTime) / 60;
          output = `${hours} Stunde${(hours==1)?'':'n'} ${restTime} Minuten`;
        } else {
          output = `${timeInMinutes} Minuten`;
        }
        return output;
      },
      myRecipes() {
        return this.$store.getters.myRecipes;
      },
      friendRecipes() {
        return this.$store.getters.friendRecipes;
      }
    },
    methods: {
      checkIfIsSaved() {
        console.log(this.showRecipe);
        var keys = Object.keys(this.myRecipes);
        for (let index = 0; index < keys.length; index++) {
          if(this.myRecipes[index].id == this.showRecipe.id) {
            this.showSaveButton = false;
            return this.showSaveButton;
          }
        }
        return this.showSaveButton;
      },
      saveFriendRecipe(recipeId) {
        var fRecipes = this.friendRecipes;
        var fKeys = Object.keys(fRecipes);
        let index, i = 0;
        for (i = 0; i < fKeys.length; i++) {
          if(fRecipes[i].id == recipeId) {
            index = i;
          }
        }
        this.$store.dispatch('addRecipe', this.friendRecipes[index]).then(() => {
          for (var prop in this.friendRecipes[index]) {
            if(prop == "categories") {
              var categories = this.friendRecipes[index][prop];
              categories.forEach((category) => {
                this.$store.dispatch('addPreferencesCategories', category.name);
              });
            }
            if(prop == "ingredients") {
              var ingredients = this.friendRecipes[index][prop];
              ingredients.forEach((ingredient) => {
                this.$store.dispatch('addPreferencesIngredients', ingredient.name);
              });
            }
            if(prop == "difficultyLevel") {
              console.log("Schwierigkeitsgrad: ", this.friendRecipes[index][prop]);
              this.$store.dispatch('addPreferencesDifficultyLevel', this.friendRecipes[index][prop]);
            }
          }
          this.$store.dispatch('removeRecipeFromFriendRecipes', index);
          this.$store.getters.prefrencedRecipes;
          this.checkIfIsSaved();  
        });
      }
    },
    mounted() {
      this.checkIfIsSaved();
    }
  }
</script>

<style>
  table {
    border-spacing: 0.5rem 1rem;
  }
  table th {
    text-align: left;
    font-weight: bold;
    color: var(--main-bg-color);
    border-bottom: 0.25rem solid var(--main-bg-color);
  }
  table td {
    text-align: left;
    vertical-align: center;
    color: var(--main-black-color);
    border-bottom: 1px dotted var(--main-grey-color);
  }
</style>