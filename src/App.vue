<template>
  <div id="app">
    <main>
      <input
        type="text"
        class="search"
        placeholder="Search city"
        v-model="query"
        @keypress="fetchWeather"
      />

      <div class="city" v-if="typeof weather.main != 'undefined'">
        <p>{{ new Date().toDateString() }}</p>
        <h2 class="city-name">
          <span>{{ weather.name }}</span>
          <sup>{{ weather.sys.country }}</sup>
        </h2>
        <div class="city-temp">
          {{ Math.round(weather.main.temp) }}<sup>&deg;C</sup>
        </div>
        <div class="info">
          <p>{{ weather.weather[0].description }}</p>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      api_key: "b98bfe3849ef13254126907a51556d94",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
      this.query = "";
    },
  },
};
</script>

<style>
body {
  font: 0.9rem sans-serif;
  background: #0a1f44;
  color: #1e2432;
  height: 100%;
  margin: 0;
}

#app {
  background: linear-gradient(rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.418)),
    url("./assets/mountain.jpg");
  background-size: cover;
  background-position: center;
}

main {
  min-height: 100vh;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.search {
  outline: none;
  padding: 20px 5%;
  border-radius: 20px;
  border: none;
  margin-bottom: 5%;
  background: rgba(250, 250, 250, 0.85);
  font-size: 1.5em;
}

.city {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  padding: 40px 8%;
  border-radius: 20px;
  background: rgba(250, 250, 250, 0.85);
  box-shadow: 10px 10px 5px 0px rgba(15, 15, 15, 0.404);
}

p {
  margin-top: 10px;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.city-temp {
  font-size: 5rem;
  font-weight: bold;
  margin-top: 10px;
  color: #1e2432;
  text-align: center;
}

.city sup {
  font-size: 0.5em;
}

.city-name {
  font-size: 2em;
}

.city-name sup {
  padding: 0.2em 0.6em;
  margin-left: 0.2em;
  border-radius: 30px;
  color: #fff;
  background: #ff8c00;
}

.city-icon {
  margin-top: 10px;
  width: 100px;
  height: 100px;
}

.info {
  display: flex;
  flex-direction: column;
  align-items: center;
}

@media only screen and (max-width: 600px) {
  .search {
    padding: 20px 10%;
  }

  .city {
    padding: 40px 20%;
  }
}
</style>