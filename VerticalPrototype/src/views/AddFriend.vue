<template>
    <div class="addfriend">
        <div class="search-container">
            <form>
                <input type="text" id="search" v-model="searchInput" required placeholder="Suche..">
                <button type="button" class="search-button" @click="searchFriends(searchInput)"><i><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><!-- Font Awesome Free 5.15.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free (Icons: CC BY 4.0, Fonts: SIL OFL 1.1, Code: MIT License) --><path d="M505 442.7L405.3 343c-4.5-4.5-10.6-7-17-7H372c27.6-35.3 44-79.7 44-128C416 93.1 322.9 0 208 0S0 93.1 0 208s93.1 208 208 208c48.3 0 92.7-16.4 128-44v16.3c0 6.4 2.5 12.5 7 17l99.7 99.7c9.4 9.4 24.6 9.4 33.9 0l28.3-28.3c9.4-9.4 9.4-24.6.1-34zM208 336c-70.7 0-128-57.2-128-128 0-70.7 57.2-128 128-128 70.7 0 128 57.2 128 128 0 70.7-57.2 128-128 128z"/></svg></i> Suchen</button>
            </form>
        </div>
        <table id="friends-output">
            <tr><th>Name</th><th>PeerID</th><th>Verbinden</th></tr>
            <tr v-for="(user, index) in searchOutput" :key="index">
                <th scope="row">{{ user.name  }}</th>  
                <td>{{ user.peerID }}</td> 
                <td v-if="myFriends.includes(user.key)">
                    <button type="button" class="primary-button"><i><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><!-- Font Awesome Free 5.15.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free (Icons: CC BY 4.0, Fonts: SIL OFL 1.1, Code: MIT License) --><path d="M256 288c79.5 0 144-64.5 144-144S335.5 0 256 0 112 64.5 112 144s64.5 144 144 144zm128 32h-55.1c-22.2 10.2-46.9 16-72.9 16s-50.6-5.8-72.9-16H128C57.3 320 0 377.3 0 448v16c0 26.5 21.5 48 48 48h416c26.5 0 48-21.5 48-48v-16c0-70.7-57.3-128-128-128z"/></svg></i> Zum Profil</button>
                    <button type="button" class="secondary-button" @click="removeAsFriend(user.key)"><i><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 640 512"><!-- Font Awesome Free 5.15.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free (Icons: CC BY 4.0, Fonts: SIL OFL 1.1, Code: MIT License) --><path d="M633.8 458.1L389.6 269.3C433.8 244.7 464 198.1 464 144 464 64.5 399.5 0 320 0c-67.1 0-123 46.1-139 108.2L45.5 3.4C38.5-2 28.5-.8 23 6.2L3.4 31.4c-5.4 7-4.2 17 2.8 22.4l588.4 454.7c7 5.4 17 4.2 22.5-2.8l19.6-25.3c5.4-6.8 4.1-16.9-2.9-22.3zM198.4 320C124.2 320 64 380.2 64 454.4v9.6c0 26.5 21.5 48 48 48h382.2L245.8 320h-47.4z"/></svg></i> Freund entfernen</button>
                </td>
                <td v-else>
                    <button type="button" class="primary-button" @click="addAsFriend(user.key)"><i><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 640 512"><!-- Font Awesome Free 5.15.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free (Icons: CC BY 4.0, Fonts: SIL OFL 1.1, Code: MIT License) --><path d="M624 208h-64v-64c0-8.8-7.2-16-16-16h-32c-8.8 0-16 7.2-16 16v64h-64c-8.8 0-16 7.2-16 16v32c0 8.8 7.2 16 16 16h64v64c0 8.8 7.2 16 16 16h32c8.8 0 16-7.2 16-16v-64h64c8.8 0 16-7.2 16-16v-32c0-8.8-7.2-16-16-16zm-400 48c70.7 0 128-57.3 128-128S294.7 0 224 0 96 57.3 96 128s57.3 128 128 128zm89.6 32h-16.7c-22.2 10.2-46.9 16-72.9 16s-50.6-5.8-72.9-16h-16.7C60.2 288 0 348.2 0 422.4V464c0 26.5 21.5 48 48 48h352c26.5 0 48-21.5 48-48v-41.6c0-74.2-60.2-134.4-134.4-134.4z"/></svg></i> Freund hinzuf&uuml;gen</button>
                </td>
            </tr>
        </table>
    </div>
</template>

<script>
import fbDatabase from '../mixins/fbDatabase';
import fbDatabaseMaintain from '../mixins/fbDatabaseMaintain';
export default {
  name: "AddFriend",
  data: function() {
    return {
      searchInput: '',
    }
  },
  computed: {
    myFriends() {
      return this.$store.getters.myFriends;
    },
    searchOutput() {
      return this.$store.getters.searchOutput;
    },
    myDatabase() {
      return this.$store.getters.myDatabase;
    },
    myKey() {
      return this.$store.getters.myKey;
    },
  },
  mixins: [fbDatabase],
    methods: {
        addAsFriend: function addAsFriend(friendKey) {
            fbDatabaseMaintain.methods.addFriend(this.myDatabase, this.myKey, friendKey).then((friends)=>{
                console.log("> New Friends: ", friends);
                this.$emit('updateFriendlist', friendKey);
            });
        },
        removeAsFriend: function removeFriend(friendKey) {
            fbDatabaseMaintain.methods.removeFriend(this.myDatabase, this.myKey, friendKey).then((friends)=>{
                console.log("> No more Friends: ", friends);
                this.$emit('updateFriendlist', friendKey);
            });
        },
        searchFriends: function searchFriends(input) {
            console.log("> You searched for: ", input);
            this.$emit('searchFriends', input);
        }
    },
  mounted() {

  }
};
</script>
<style>
    #friends-output{
        width: 100%;
        text-align: center;
        justify-items: center;
        align-items: center;
    }
</style>