<template>
  <div id="app">
    <main>
      <link rel="stylesheet" type="text/css" href="//fonts.googleapis.com/css?family=Nothing+You+Could+Do" />
      <div class="sumMonth">{{ timeTillSummer() }}
        <p id="daystext"> </p>
        <p id="hours"> </p>
        <h2 id="end"></h2>
      </div>
      <div id="logo">
        <img src="./assets/logo.svg">
      </div>
      <div id="logo2">
        <img src="./assets/facebook-icon.svg">
      </div>
      <div id="monthsofsum" class="hero-text"> How many months of Summer <br> have you lived through? </div>
      <div class="age-container">
        <input type="number" class="search-box" placeholder="Your age" name="numberInput" id="numberInput" min="1"
          step="1">
        <button type="button" class="calculate-age-button" v-on:click="calcAge()">Calculate Age</button>
      </div>
      <div class="age-container">
        <div class="weather-at-bolser">
          <div class="bolser-weather-text">
            What's the weather like <br> at Bolser Agency?
          </div>
          <button class="show-me-button" v-on:click="fetchWeather()">show me</button>
        </div>
      </div>
      <div class="bottomContainer">
        <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
          <div class="current">Currently:<br> {{ weather.weather[0].main }}</div>
          <div class="icon"><img :src="getIconUrl(weather.weather[0].icon)"></div>
          <div class="weather-box">
            <div class="weather-top-row">
              <div class="weather">temp: {{ Math.round(weather.main.temp) }}°c </div>
              <div class="weather">visibility: {{ convertMeterstoKm(weather.visibility) }}</div>
              <div class="weather">Rain: {{ rainInMM(Math.round(weather.rain)) }} mm</div>
            </div>
            <div class="weather-bottom-row">
              <div class="weather">Wind: {{ Math.round(convertMetersPerSec(weather.wind.speed)) }}mph </div>
              <div class="weather">Wind Dir: {{ weatherdir(weather.wind.deg) }}</div>
              <div class="weather">humidity: {{ Math.round(weather.main.humidity) }}%</div>
            </div>
          </div>
        </div>
      </div>
    </main>
    <div class="allrows">
      <div class="row0"></div>
      <div class="row1"></div>
      <div class="row2"></div>
      <div class="row3"></div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  // 
  data() {
    return {
      api_key: 'e11d310fcdddf4869a0c0753ae9ad22f',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      location: 'lon=-1.553720&lat=53.791710',
      weather: {}
    }
  },
  methods: {
    fetchWeather() {
      {
        fetch(`${this.url_base}/weather?${this.location}&units=metric&appid=${this.api_key}`)
          .then(res => {
            return res.json();

          }).then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },

    timeTillSummer() {
      var countDownDate = new Date("Sep 21, 2022 00:00:00").getTime();
      var myfunc = setInterval(function () {

        var now = new Date().getTime();
        var timeleft = countDownDate - now;

        // Calculating the days and hours left
        var days = Math.floor(timeleft / (1000 * 60 * 60 * 24));
        var hours = Math.floor((timeleft % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));

        // Result is output to the specific element
        var output = ""
        output += days + " days, " + hours + " hours" + "<br>" + " of summer to go!"
        document.getElementById("daystext").innerHTML = output

        // Display the message when countdown is over
        if (timeleft < 0) {
          clearInterval(myfunc);
          document.getElementById("end").innerHTML = "Summer is over!";
        }
      }, 0);
    },
    rainInMM(rain) {
      // TODO fix
      //if rain appears NAN then display 0mm
      rain = rain || 0
      return rain
    },

    getIconUrl(iconId) {
      return `https://openweathermap.org/img/wn/${iconId}@2x.png`
    },

    convertMetersPerSec(mps) {
      const mph = 2.23694 * mps
      return mph;
    },

    convertMeterstoKm(meters) {
      const km = meters / 1000
      return km
    },

    weatherdir(degree) {
      {
        var val = Math.floor((degree / 22.5) + 0.5);
        var arr = ["N", "NNE", "NE", "ENE", "E", "ESE", "SE", "SSE", "S", "SSW", "SW", "WSW", "W", "WNW", "NW", "NNW"];
        return arr[(val % 16)];
      }
    },

    calcAge() {
      // calculates age with consideration to current year
      let age = document.getElementById("numberInput").value - 1;
      let d = new Date();
      let currentMonth = d.getMonth();

      console.log('current month is ' + currentMonth)

      var output = ""
      var summerMonthsLived = age * 3
      if (currentMonth > 8 || currentMonth == 7) {
        summerMonthsLived += 3
      } else if (currentMonth == 6) {
        console.log(summerMonthsLived)
        summerMonthsLived += 2
      } else if (currentMonth == 5) {
        summerMonthsLived += 1
      }
      output += "you've lived through " + summerMonthsLived + " months of summer"

      document.getElementById("monthsofsum").innerHTML = output
      console.log(output)
    }
  }

}


</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Nothing You Could Do";
  font-size: 35px;
  font-weight: bold;

}

#app {
  background-image: url('./assets/hero-background.jpg'), url('./assets/body-background.jpg');
  background-size: 100% 50%, 100% 50%;
  background-repeat: no-repeat, no-repeat;
  background-position: top, bottom;
  background-attachment: fixed;
  z-index: -1;

}

#logo {
  position: absolute;
  top: 8px;
  left: 18px;
  font-size: 7px;
  background-attachment: fixed;
  margin-left: 2px;
  margin-top: 2px;
}

#logo2 {
  position: absolute;
  top: 8px;
  right: 16px;
  font-size: 4px;
  background-attachment: fixed;
  margin-right: 2px;
  margin-top: 2px;
}

/* 
using rgba for transparency of background without text
converted hex codes to rgbas
other one = rgba(212, 202, 88, 50%)
FF7142 = rgb(255, 113, 66)
*/

main {
  background-image: linear-gradient(to bottom, rgba(212, 202, 88, 50%), rgba(255, 113, 66, 50%)), linear-gradient(to bottom, rgba(212, 202, 88, 50%), rgba(255, 113, 66, 50%));
  background-attachment: fixed;
  background-size: 100% 50%, 100% 50%;
  background-repeat: no-repeat, no-repeat;
  background-position: top, bottom;
  min-height: 100vh;
  /* opacity: 50%; */
}

.text {
  margin-top: 30px;
  text-align: center;
  font-family: "Palatino Linotype", "Book Antiqua", Palatino, serif;
  font-size: 15px;
  letter-spacing: 1.2px;
  word-spacing: 2px;
  color: #000000;
  font-weight: 400;
  text-decoration: none solid rgb(68, 68, 68);
  font-style: normal;
  font-variant: normal;
  text-transform: none;
}


.calculate-age-button {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  color: rgb(255, 255, 255);
  background-color: rgba(255,255,255, 0.3);
  font-size: 18px;
  line-height: 16px;
  padding: 22px;
  border-radius: 0px;
  font-family: "Nothing You Could Do";
  font-weight: 900;
  text-decoration: none;
  font-style: normal;
  font-variant: normal;
  text-transform: none;
  box-shadow: rgba(0, 0, 0, 0) 100px 100px 0px 0px;
  border: 3px solid rgb(255, 255, 255);
  display: inline-block;
}

.calculate-age-button:hover {
  background: rgb(255, 175, 175);
}

.calculate-age-button:active {
  background: rgb(170, 170, 170);
}

.weather-at-bolser {
  color: rgb(255, 255, 255);
  font-size: 18px;
  padding-right: 10px;
  margin-top: 2%;
  margin-right: 10%;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
}


.show-me-button {
  justify-content: center;
  padding-left: 10px;
  align-items: center;
  text-align: center;
  color: rgb(255, 255, 255);
  background-color: rgba(255,255,255, 0.3);
  font-size: 28px;
  line-height: 16px;
  padding: 22px;
  border-radius: 0px;
  font-family: "Nothing You Could Do";
  font-weight: 1300;
  text-decoration: none;
  font-style: normal;
  font-variant: normal;
  text-transform: none;
  box-shadow: rgba(0, 0, 0, 0) 100px 100px 0px 0px;
  border: 3px solid rgb(255, 255, 255);
  display: inline-block;
}

.show-me-button:hover {
  background: rgb(255, 175, 175);
}

.show-me-button:active {
  background: rgb(170, 170, 170);
}

.bottomContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  margin-top: 1em;
}

.sumMonth {
  padding-top: 2em;
  margin-bottom: 10em;
  color: #FFF;
  font-size: 43px;
  font-weight: 600;
  text-align: center;
}

.hero-text {
  color: rgb(255, 255, 255);
  font-size: 25px;
  font-weight: 1400;
  text-align: center;
  margin-top: 10px;
  margin-bottom: 10px;
}


.search-box {
  margin-right: 10px;
  font-family: "Nothing You Could Do";
  color: white;
  border: 2px solid white;
  background: transparent;
  width: 15%;
  padding: 16px;
  margin-bottom: 20px;
  font-size: 20px;
  line-height: 20px;
  min-width: 7em;
}

.search-box::placeholder { /* Chrome, Firefox, Opera, Safari 10.1+ */
  color: lightgray;
  font-weight: bold;
}


.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.current {
  left: 25%;
  color: #FFF;
  font-size: 22px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);

}

.weather-box {
  text-align: center;
}

.weather-box .weather {
  padding-left: 20px;
  display: inline-block;
  color: #FFF;
  font-size: 22px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.allrows {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 50%;
  display: grid;
  grid: repeat(1, 12vw) / auto-flow
}

.row0 {
  background-color: #CBBEBE;
}

.row1 {
  background-color: #f0f1dd;
}

.row2 {
  background-color: #CBBEBE;
}

.row3 {
  background-color: #f0f1dd;
}

.weather-wrap {
  display: flex;
}

.weather-top-row {
  background-color: rgba(203, 190, 190, 55%)
}

.weather-bottom-row {
  background-color: rgba(240, 241, 221, 55%);
  margin-top: 10px;
  padding-left: 1em;
  padding-right: 1em;
}

.age-container {
  text-align: center;
}

.bolser-weather-text {
  margin-right: 10px;
  padding: 15px;
}
</style>
