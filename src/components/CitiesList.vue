<template>
  <div class="hello">
    <h1 v-if="cities.length > 1">{{ msg }}</h1>
    <h3 v-if="cities.length > 0">Select Your City</h3>
    <ul v-for="city in cities" :key="city.id">
      <li>
        <a @click="selectedCity(city)" href="#" target="_self" rel="noopener">
          {{city.name}}, {{city.sys.country}}
        </a>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'CitiesList',
  props: {
    msg: String,
    cities: Array
  },
  methods: {
    selectedCity(selCity) {
      this.$emit('select-city', selCity);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #208be2;
}
</style>
