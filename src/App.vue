<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && 26 >= weather.main.temp >= 16
        ? 'normal'
        : typeof weather.main != 'undefined' && weather.main.temp < 16
        ? 'cold'
        : typeof weather.main != 'undefined' && weather.main.temp > 26
        ? 'warm'
        : ''
    "
  >
    <main
      :class="
        typeof weather.main != 'undefined' &&
        weather.weather[0].main == 'Clouds'
          ? 'clouds'
          : typeof weather.main != 'undefined' &&
            weather.weather[0].main == 'Snow'
          ? 'snow'
          : typeof weather.main != 'undefined' &&
            weather.weather[0].main == 'Rain'
          ? 'rain'
          : typeof weather.main != 'undefined' &&
            weather.weather[0].main == 'Haze'
          ? 'haze'
          : ''
      "
    >
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div id="app-intro">
        <h1>This is a weather app.</h1>
        <h3>Enter a city name in the search field.</h3>
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }} °C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "995387fcb5bc094ed802de8105ccd9f1",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    fetchWeather(e) {
      document.getElementById("app-intro").style.display = "none";
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
        "October",
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
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Righteous&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Righteous", cursive;
  letter-spacing: 3px;
}

#app {
  background-image: url("./assets/bg/normal-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
  .cold {
    background-image: url("./assets/bg/cold-bg.jpg");
    background-position: bottom;
  }
  .warm {
    background-image: url("./assets/bg/warm-bg.jpg");
    background-position: bottom;
  }
}

#app-intro {
  color: #fff;
  text-align: center;
  margin-top: 10vh;
  h1 {
    font-size: 3rem;
    margin-bottom: 0.7em;
  }
}

main {
  min-height: 100vh;
  padding: 25px;
  background-size: cover;
  opacity: 0.8;
}

main.clouds {
  background-image: url("./assets/weather-effects/clouds.png");
  opacity: 1;
}
main.snow {
  background-image: url("./assets/weather-effects/snow.png");
}
main.rain {
  background-image: url("./assets/weather-effects/rain.png");
}
main.haze {
  background-image: url("./assets/weather-effects/haze.png");
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
  .search-bar {
    display: block;
    width: 100%;
    padding: 15px;
    color: #313131;
    font-size: 20px;
    appearance: none;
    border: none;
    outline: none;
    box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 0px 16px;
    transition: 0.4s;
    &:focus {
      box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
      background-color: rgba(255, 255, 255, 0.75);
      border-radius: 16px 0px;
    }
  }
}

.location-box {
  .location {
    color: #fff;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 3px 6px rgb(0, 0, 0, 0.75);
  }
  .date {
    color: #fff;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: center;
  }
}

.weather-box {
  text-align: center;
  .temp {
    display: inline-block;
    padding: 10px 25px;
    color: #fff;
    font-size: 102px;
    font-weight: 900;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.75);
    background-color: rgba(0, 0, 0, 0.45);
    border-radius: 16px;
    margin: 30px 0px;
    box-shadow: 3px 6px rgba(0, 0, 0, 0.75);
  }

  .weather {
    color: #fff;
    font-size: 48px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.75);
  }
}

.none {
  display: none;
}

.show {
  display: block;
}
</style>
