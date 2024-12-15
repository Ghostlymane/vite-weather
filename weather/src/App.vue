<script>

export default {
  data() {
    return {
      location: '',
      temperature: 0,
      description: '',
      loading: false,
      error: false,
      searchQuary: '',
    };
  },
  computed: {
    weatherClass() {
      if (this.description.includes('Sunny')) {
        return 'sunny'
      } else if (this.description.includes('Overcast')) {
        return  'Overcast'
      } else if (this.description.includes('Partly cloudy')) {
        return 'partly-Cloudy'
      } else {
        return '';
      }
    }
  },
  methods: {
    weatherSearch() {
      this.loading = true
      this.error = false
      fetch(`http://api.weatherapi.com/v1/current.json?key=44d9cb0614284c4cbce80041241512&aqi=no&q=${this.searchQuary}`)
          .then(response => response.json())
          .then(data => {
              this.loading = false
              this.location = data.location.name
              this.temperature = data.current.temp_c
              this.description = data.current.condition.text
              this.resetSearchQuery()
          })
          .catch(error => {
            this.loading = false
            this.error = true
            console.error(error)
          });
    },
    resetSearchQuery() {
      this.searchQuary = ''
    }
  }
}
</script>

<template>
  <div class="weather" :class="weatherClass">
    <div class="container">

      <div class="card weather-form">
        <input type="text" class="weather-form_input"
        v-model="searchQuary" @keyup.enter="weatherSearch" placeholder="Введите город">
        <button class="weather-form_btn" @click="weatherSearch">Поиск</button>
      </div>

      <div class="card weather-load" v-if="loading">Загрузка...</div>

      <div class="weather-info" v-show="!error && location && temperature !== 0 && description">

        <div class="card" v-if="error">Error</div>

        <div class="weather-info_text">
          <p class="card">{{ location }}</p>
          <p class="card">{{ temperature }}°C</p>
          <p class="card">{{ description }}</p>
        </div>
      </div>

    </div>

    <div class="weather-bg">
      <div>
        <img src="./assets/moonLight.jpg" alt="App Background" class="weather-bg_img bg">
        <img src="./assets/darkClouds.jpg" alt="App Background" class="weather-bg overcast">
        <img src="./assets/blueClouds.jpg" alt="partly Cloudy" class="weather-bg partly-Cloudy">
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
