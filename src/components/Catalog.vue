<template>
  <div class="container">
    <div class="catalog">
      <Card v-for="(item, i) in beer" :key="i" :index="i" :data="item" @deleteEvent="deleteBeer" @editEvent="editBeer"/>
    </div>

    <div class="pagination">
      <button class="next" v-if="showButton" @click="showNextList">Show next >></button>
    </div>
  </div>

  <div class="modal" v-if="showModal">
    <div class="content">
      <input type="text" name="name" placeholder="Enter product name" v-model="beer[this.beerIndex].name">
      <input type="text" name="name" placeholder="Enter product description" v-model="beer[this.beerIndex].description">

      <button type="submit" @click="saveBeer">Save</button>
      <button type="button" data-dismiss="modal" @click="editCancel">Cancel</button>
    </div>
  </div>
</template>

<script>
import Card from './Card.vue'
const axios = require('axios');

export default {
  name: "Catalog",
  components: {
    Card
  },
  data() {
    return {
      beer: [],
      addBeer: [],
      beerIndex: 0,
      showModal: false,
      oldName: '',
      oldDescription: '',
      showButton: true,
      counter: 1
    };
  },

  mounted() {
    axios
        .get('https://api.punkapi.com/v2/beers?page=1&limit=25')
        .then(response => (this.beer = response.data))
        .catch(error => console.log(error));
  },

  methods: {
    deleteBeer(index) {
      this.beer.splice(index, 1);
    },

    editBeer(index) {
      this.showModal = true;
      this.beerIndex = index;
      this.oldName = this.beer[index].name;
      this.oldDescription = this.beer[index].description;
    },

    saveBeer() {
      this.showModal = !this.showModal;
    },

    editCancel() {
      this.beer[this.beerIndex].name = this.oldName;
      this.beer[this.beerIndex].description = this.oldDescription;
      this.showModal = !this.showModal;
    },

    showNextList() {
      axios
          .get('https://api.punkapi.com/v2/beers?page=' + this.counter + '1&limit=25')
          .then(response => {
              this.addBeer = response.data;
              this.counter += 1;
              this.beer = this.beer.concat(this.addBeer);
              if (this.addBeer.length === 0) {
                this.showButton = false
              }
            }
          )
          .catch(
              error => console.log(error),
          );
    }
  }
}
</script>

<style scoped>
  .catalog {
    display: grid;
    grid-gap: 20px;
    grid-template-columns: repeat(3, 1fr);
  }

  .container {
    margin: 30px auto;
    width: 80vw;
  }

  .pagination {
    margin-top: 30px;
  }

  .modal {
    background: #00000094;
    display: flex;
    height: 100vh;
    position: fixed;
    top: 0;
    width: 100vw;
  }

  .content {
    background: #fff;
    margin: auto;
    padding: 20px;
    width: 30vw;
  }

  .content input {
    box-sizing: border-box;
    display: block;
    height: 30px;
    margin-bottom: 10px;
    width: 100%;
  }

  .content button {
    margin-right: 10px;
  }
</style>