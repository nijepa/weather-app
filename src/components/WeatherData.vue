<template>
  <div class="con">
    <h1><span>{{ weatherData.name }}, {{ weatherData.sys.country }}</span></h1>
    <h3>Current Weather : <span>{{ weatherData.weather[0].main }} - {{ weatherData.weather[0].description }}</span></h3>
    <img class="pics" :src="'http://openweathermap.org/img/w/' + weatherData.weather[0].icon + '.png'" alt="">
    <h3>Feels Like : <span>{{ weatherData.main.feels_like }} &#8451;</span> </h3>
    <h5>Temperature : <span>{{ weatherData.main.temp }} &#8451;</span> </h5>
    <h5>Max Temp. : <span>{{ weatherData.main.temp_max }} &#8451;</span> </h5>
    <h5>Min Temp. : <span>{{ weatherData.main.temp_min }} &#8451;</span> </h5>
    <h5>Humidity : <span>{{ weatherData.main.humidity }} %</span></h5>
    <h5>Pressure : <span>{{ weatherData.main.pressure }} mb</span></h5>
    <h5>Wind : <span>{{ weatherData.wind.speed }} km/h</span></h5>
    <h5 v-if="weatherData.rain">Rain : <span>{{ weatherData.rain }}</span></h5>
    <h5 v-if="weatherData.snow">Snow : <span>{{ weatherData.snow }}</span></h5>
  </div>
</template>

<script>
export default {
  name: 'WeatherData',
  props: {
    weatherData: Object
  }
}
</script>

<style lang="scss" scoped>
  .pics {
    width: 120px;
    height: auto;
  }
  span {
    color:royalblue;
  }
</style>>