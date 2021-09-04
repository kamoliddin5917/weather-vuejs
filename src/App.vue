<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' &&
      (weather.main.temp > 40 ? 'warm' : weather.main.temp < 0 ? 'cold' : '')
    "
  >
    <main>
      <div class="error" v-if="weather.cod == 404">{{ weather.message }}!</div>

      <div class="time" v-if="weather.timezone ? false : false">
        {{ time() }}
      </div>

      <div class="search-box">
        <input
          class="search-bar"
          type="text"
          placeholder="Search..."
          v-model="query"
          v-on:keypress="showWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].description }}</div>
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
      api_key: "66a5d7418e3b615ac32b4a539746659b",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },

  methods: {
    showWeather(e) {
      if (e.key === "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => res.json())
          .then(this.setResults);
      }
    },

    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "Oktober",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
    time() {
      let v = new Date().getTimezoneOffset(this.weather.timezone);
      return v;
    },
  },
};
</script>

<style>
*,
* ::after,
* ::before {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
html {
  scroll-behavior: smooth;
}
body {
  font-family: "montserrat", sans-serif;
}
#app {
  background-image: url("./assets/sunny.jpg");
  background-size: cover;
  background-repeat: no-repeat;
  background-position: bottom;
  transition: 0.4s;
}
#app.warm {
  background-image: url(./assets/desert.jpg);
}
#app.cold {
  background-image: url(./assets/cold.jpg);
}
main {
  position: relative;
  display: grid;
  grid-template-columns: 50% 50%;
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(255, 255, 255, 0.3),
    rgba(0, 0, 255, 0.6)
  );
}
.search-box {
  width: 90;
}
.search-box .search-bar {
  margin-top: 30vh;
  display: block;
  width: 100%;
  padding: 15px;
  color: gold;
  text-shadow: -1px 1px 4px #000;
  font-weight: 700;
  font-size: 20px;
  letter-spacing: 2px;
  border: none;
  outline: none;
  background: none;
  appearance: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0 16px 0 16px;
  transition: 0.4s;
}
.search-box .search-bar::placeholder {
  text-shadow: 0 0 0 #000;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location {
  margin-top: 10vh;
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 100px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  box-shadow: rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0;
}
.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: rgba(0, 0, 0, 0.25);
}
.error {
  position: absolute;
  top: 2vh;
  left: 50%;
  transform: translateX(-50%);
  color: red;
  font-size: 30px;
  font-weight: 600;
  font-style: italic;
  text-shadow: 2px 2px 2px gold;
  text-transform: uppercase;
}
.time {
  position: absolute;
  font-size: 40px;
  font-weight: 900;
}
@media only screen and(max-width: 600px) {
  main {
    grid-template-columns: 100%;
  }

  .search-box .search-bar {
    margin-top: 10vh;
  }

  .location-box .location {
    margin-top: 5vh;
  }
}
</style>
