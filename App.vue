<template>
  <div class="weather" :class="weatherClass">
    <div class="container">
      <div class="card weather-form">
        <input
          type="text"
          class="weather-form_input"
          v-model="searchQuery"
          @keyup.enter="weatherSearch"
          placeholder="Enter city"
        />
        <button class="weather-form_btn" @click="weatherSearch">Search</button>
      </div>
      <div class="card weather-load" v-if="loading">Loading...</div>
      <div
        class="weather-info"
        v-show="!error && location && temperature !== 0 && description"
      >
        <div class="card" v-if="error">Error</div>
        <div class="weather-info_text">
          <p class="card">{{ location }}</p>
          <p class="card">{{ temperature }}°C</p>
          <p class="card">{{ description }}</p>
        </div>
      </div>
    </div>
    <div class="weather-bg">
      <img
        class="weather-bg_img bg"
        src="./assets/planet.jpg"
        alt="App background"
      />
      <img
        class="weather-bg_img overcast"
        src="./assets/overcast.jpg"
        alt="App background"
      />
      <img
        class="weather-bg_img partly-cloudy"
        src="./assets/partly_cloudy.jpg"
        alt="App background"
      />
      <img
        class="weather-bg_img sunny"
        src="./assets/fon11.jpg"
        alt="App background"
      />
      <img
        class="weather-bg_img light-rain"
        src="./assets/light_rain.jpg"
        alt="App background"
      />

      <img
        class="weather-bg_img heavy-rain"
        src="./assets/heavy_rain.jpg"
        alt="App background"
      />
      <img
        class="weather-bg_img moderate-or-heavy-rain-with-thunder"
        src="./assets/thunder.jpg"
        alt="App background"
      />
      <img
        class="weather-bg_img moderate-or-heavy-rain-shower"
        src="./assets/rain.jpg"
        alt="App background"
      />
      <img
        class="weather-bg_img mist"
        src="./assets/mist.jpg"
        alt="App background"
      />
    </div>
    <div class="copy">
      AlexDev
      <a href="mailto:alex_stranger_dev@protonmail.com"
        >| alex_stranger_dev@protonmail.com</a
      >
    </div>
  </div>
</template>

<script>
export default {
  name: "Weather",
  data() {
    return {
      location: "",
      temperature: 0,
      description: "",
      loading: false,
      error: false,
      searchQuery: "",
    };
  },
  computed: {
    weatherClass() {
      if (this.description.includes("Sunny")) {
        return "sunny";
      } else if (this.description.includes("Overcast")) {
        return "overcast";
      } else if (this.description.includes("Partly cloudy")) {
        return "partly-cloudy";
      } else if (this.description.includes("Light rain")) {
        return "light-rain";
      } else if (this.description.includes("Heavy rain")) {
        return "heavy-rain";
      } else if (
        this.description.includes("Moderate or heavy rain with thunder")
      ) {
        return "moderate-or-heavy-rain-with-thunder";
      } else if (
        this.description.includes("Moderate or heavy rain shower")
      ) {
        return "moderate-or-heavy-rain-shower";
        
      } else if (this.description.includes("Mist")) {
        return "mist";
      } else {
        return "";
      }
    },
  },

  methods: {
    weatherSearch() {
      this.loading = true;
      this.error = false;
      fetch(
        `http://api.weatherapi.com/v1/current.json?key=5a94c3d2e7b74110a58151816243105&q=${this.searchQuery}`
      )
        .then((response) => response.json())
        .then((data) => {
          this.loading = false;
          this.location = data.location.name;
          this.temperature = data.current.temp_c;
          this.description = data.current.condition.text;
          this.resetSearchQuery();
        })
        .catch((error) => {
          this.loading = false;
          this.error = true;
          console.error(error);
        });
    },
    resetSearchQuery() {
      this.searchQuery = "";
    },
  },
};
</script>
