<template>
  <div id="app">
      <div class="form-group">
        <label for="title">City Name : </label>
        <input type="text" placeholder="pls enter your city name" 
                name="title" id="title" v-model="city" required>
      </div>
      <div class="form-btns">
        <button id="search" @click="getW(city)">Search</button>
      </div>
      <cities-list v-if="cities && status != 'found'" @select-city="handleCity" 
                    :cities="cities" msg="Found Cities List :"/>
    <div v-if="status == 'no'" class="loading">Nothing found :( Try again</div>                    
    <div v-if="status == 'load'" class="loading">loading ...</div>
    <div class="container" v-if="myCity.name">
      <img class="responsive" alt="Vue logo" :src="imgSrc">
      <weather-data :weatherData="myCity" />
    </div>
  </div>
</template>

<script>
import CitiesList from './components/CitiesList.vue'
import WeatherData from './components/WeatherData.vue'

export default {
  name: 'App',
  components: {
    CitiesList, WeatherData
  },
  data() {
    return {
      cities: [],
      city: '',
      imgSrc: '',
      myCity: {},
      status: ''
    }
  },

  mounted() {
    if (!localStorage.getItem('city')) {
      this.populateStorage();
    } else {
      this.getStorage();
    }
    Promise.all([this.getW(this.city), this.getImg(this.city)])
      .catch((err) => {
        console.log(err)
      })
  },

  methods: {
    populateStorage() {
      localStorage.setItem('city', JSON.stringify(this.city));
    },

    getStorage() {
      if (!localStorage.getItem('city')) {
        this.populateStorage();
      }
      this.city = JSON.parse(localStorage.getItem('city'));
      return this.city;
    },

    handleCity(selCity) {
      this.status = 'busy';
      this.myCity = selCity;
      this.populateStorage();
      if (selCity) {
        this.getImg(selCity.name);
        this.status = 'found';
      } else {
        this.imgSrc = '';
        this.status = 'no';
        this.myCity = {};
      }
      
    },

    async getW(city)  {
      this.status = 'busy';
      const response = await fetch(`http://api.openweathermap.org/data/2.5/find?q=${city}&units=metric&APPID=0386e768e5bab2c1a881400231ed2f19`, {mode: 'cors'})
      const wData = await response.json()
      .then((data) => {
        if (data.cod == '200') {
          this.cities = data.list;
          this.status= ''
        } else {
          this.cities = [];
          this.status = 'no';
          this.imgSrc = '';
          this.myCity = {};
        }
      });
/*       .then((err) => {
        console.log(err)
      }); */
console.log(wData)
      //this.cities = wData.list;
    },

    async getImg(city) {
      const response = await fetch(`https://api.teleport.org/api/urban_areas/slug:${city.toLowerCase()}/images/`, {mode: 'cors'})
            //const response = await fetch(`https://api.teleport.org/api/cities/?search=${city.toLowerCase()}/images/`, {mode: 'cors'})
      const imgData = await response.json()
      .then((data) => {
        if (data) {
          this.imgSrc = data.photos[0].image.web;
        } else {
          this.imgSrc = '';
        }
      });
      console.log(imgData)
      //this.imgSrc = imgData.photos[0].image.web;
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Supermercado One', cursive;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#search{
  cursor: pointer;
  margin: .5rem;
  font-family: 'Supermercado One', cursive;
  font-size: 2rem;
  background-color: #2c3e50;
  color: white;
  border: 1px solid white;
  border-radius: .5rem;
}
#search:hover {
  background-color: white;
  color: #2c3e50;
}
.responsive {
  width: 100%;
  //max-width: 800px;
  height: auto;
}
</style>
