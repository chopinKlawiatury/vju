<template>
  <div class="wrapper">
    <div class="home">
      <label for="search">Search</label>
      <input type="text"
             name="search"
             v-model="searchValue"
             @input="handleInput">
      <div class="list" v-for="item in results" :key="item.data[0].nasa_id" >
        {{ item.data[0].description }}
      </div>
    </div>
  </div>
</template>

<script>
/* eslint-disable */

import axios from 'axios';
import debounce from 'lodash.debounce';

    const API = 'https://images-api.nasa.gov';
    export default {
        name: 'Home',
        data(){
            return{
              searchValue:'',
                results: [],
            }
        },

        methods:{
            handleInput: debounce(function() {
            axios.get(`${API}/search?q=${this.searchValue}&media_type=image`)
                .then((response) => {
                    this.results = response.data.collection.items;
                    console.log('result',this.results);
                });
          },500),
        },
    };
</script>

<style lang="scss" scoped>
.wrapper{
  width: 92%;
  margin:0 auto;

  .home{
    display: flex;
    flex-direction:column;
    max-width: 250px;
    width:100%;
    margin:0 auto;
  }

  input{
    height:30px;
    border:0;
    border-bottom: 1px solid black;
  }

  label{
    font-family: 'Montserrat', sans-serif;
  }
}
</style>