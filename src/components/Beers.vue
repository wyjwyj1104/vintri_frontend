<template>
  <div class="container">
    <div>
      <label class="title"> Beers: </label>
      <input type="text" v-focus @input="search">
    </div>
    <table class="table">
      <thead>
        <tr>
          <th scope="col">Name</th>
          <th style="width:300px" scope="col">Description</th>
          <th scope="col">First Brewed</th>
          <th scope="col">Food Pairing</th>
          <th scope="col">Rating</th>
        </tr>
      </thead>
      <tbody>
        <template v-for="beer in beers" v-bind:key="beer.id">
          <tr>
            <th scope="row">{{beer.name}}</th>
            <td style="text-align:left">{{beer.description}}</td>
            <td>{{beer.first_brewed}}</td>
            <td>
              <template v-for="(food, i) in beer.food_pairings" v-bind:key="i">
                {{food}}<br>
              </template>
            </td>
            <input
                type="number"
                min="1" max="5"
                v-if="beer.edit"
                @blur="beer.edit = false; $emit('update')"
                @keyup.enter="updateRating(beer, $event.target.value); beer.edit=false; $emit('update')"
                v-focus
            >
            <template v-else>
                <td @click="beer.edit = true;"> {{beer.rating}} </td>
            </template>
          </tr>
        </template>
      </tbody>
    </table>
  </div>
</template>
<script>
  import axios from 'axios';

  export default {
    td: '#app',
    name: 'Beers',
    data() {
      return {
        beers: null
      };
    },
    created: function() {
      axios
        .get('http://localhost:3000/beers/all', {
          auth:{
            username: 'admin',
            password: 'qwe123'
          }
        })
        .then(res => {
          if (res.data.length > 0) {
            this.beers = res.data;
            this.beers = this.beers.map(obj=> ({ ...obj, edit: false }))
          }
        })
    },
    methods: {
      search: function(evt){
      console.log("evt.target.value:", evt.target.value)
        axios
          .get('http://localhost:3000/beers?name='+evt.target.value, {
            headers:{
              'x-user': 'admin@gmail.com',
              'Content-Type' : 'application/json',
              'Accept' : 'application/json',
              'Authorization' : 'Basic admin:qwe123'
            },
            auth:{
              username: 'admin',
              password: 'qwe123'
            }
          })
          .then(res => {
          console.log("res.data:", res.data)
            if (res.data.length > 0) {
            console.log("res.data:", res.data)
              this.beers = res.data;
              this.beers = this.beers.map(obj=> ({ ...obj, edit: false }))
              this.$forceUpdate();
            }
          })
      },
      updateRating: function (beer, rating) {
        beer.rating = rating;
        axios
          .put('http://localhost:3000/beers/'+ beer._id +'?rating='+rating, {}, {
            headers:{
              'x-user': 'admin@gmail.com',
              'Content-Type' : 'application/json',
              'Accept' : 'application/json',
              'Authorization' : 'Basic admin:qwe123'
            },
            auth:{
              username: 'admin',
              password: 'qwe123'
            }
          })
          .then();
          this.$forceUpdate();
      }
    },
    directives: {
      focus: {
        inserted (td) {
          td.focus()
        }
      }
    }
  }
</script>
<style>
  .title {
    margin-right: 10px;
    margin-bottom: 5%;
    font-size: 25px;
    font-weight: bold;
  }
  .vuetable th#_Description {
    width: 80px;
  }
</style>
