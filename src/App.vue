<template>
  <div
    class="min-vh-100 d-flex justify-content-center align-items-center bg-light"
  >
    <div class="container">
      <div class="row justify-content-center">
        <div class="col-12 col-lg-5">
          <div class="box p-4 rounded text-white text-center shadow">
            <h2 class="mb-4">🌤️ Weather App</h2>

            <div class="input-group mb-3">
              <input
                v-model="city"
                @keyup.enter="getWeather"
                type="text"
                class="form-control"
                placeholder="Enter city name"
              />
              <button class="input-group-text" @click="getWeather">
                <i class="fas fa-search"></i>
              </button>
            </div>

            <div v-if="loading" class="text-center mt-3">
              <div class="spinner-border text-light" role="status"></div>
            </div>

            <div v-if="error" class="alert alert-danger mt-3">
              {{ error }}
            </div>

            <div v-if="weather" class="mt-3">
              <h1>{{ Math.round(weather.main.temp) }}°C</h1>
              <h2>{{ weather.name }}</h2>
              <p class="weather-status">
                {{ weather.weather[0].description }} ☁️
              </p>

              <div class="d-flex justify-content-between mt-4 px-2">
                <div class="d-flex align-items-center">
                  <i class="fas fa-tint me-2"></i>
                  <span>Humidity: {{ weather.main.humidity }}%</span>
                </div>
                <div class="d-flex align-items-center">
                  <i class="fas fa-wind me-2"></i>
                  <span>Wind: {{ weather.wind.speed }} m/s</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      city: "",
      weather: null,
      error: "",
      loading: false,
      apiKey: "dcce7dded8bc7085551979ade9e5b0a3",
    };
  },
  methods: {
    async getWeather() {
      if (!this.city) return;
      this.weather = null;
      this.error = "";
      this.loading = true;

      try {
        const response = await fetch(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${this.apiKey}&units=metric`
        );
        const data = await response.json();

        if (response.ok) {
          this.weather = data;
        } else {
          this.error = "City not found, please try again.";
        }
      } catch (err) {
        this.error = "Failed to fetch weather data.";
      } finally {
        this.loading = false;
      }
    },
  },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

.box {
  background: linear-gradient(to bottom, #3ac7e0, #0096c7);
  min-height: 175px;
}

.form-control:focus {
  box-shadow: none !important;
  border-color: #00b4d8;
}

.weather-status {
  font-size: 1.2rem;
  font-weight: 500;
  margin-top: 10px;
}
</style>
