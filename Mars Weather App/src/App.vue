<template>
  <div id="app">
    <button class="button" v-on:click="changeShow($event)">
      <div v-if="show === false">See Astronomy Picture of the Day</div>
      <div v-else-if="show === true">See Weather Info</div>
    </button>

    <div v-if="show === true" id="apod">
      <img
        src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/02/Transparent_square.svg/120px-Transparent_square.svg.png"
        style="
          height: 500px;
          width: 500px;
          border: 0;
          outline: none;
          outline-style: none;
        "
      />
    </div>
    <Title v-if="show === false" />

    <div v-if="show === false" id="displayContainer">
      <div id="displayNormalDays" v-for="n in daysToDisplay - 1" :key="n">
        <DayDisplay
          :sol="sols[n - 1]"
          :tempMax="tempMax[n - 1]"
          :tempMin="tempMin[n - 1]"
          :windSpeed="windSpeed[n - 1]"
          :atmosP="atmosP[n - 1]"
          :date="dates[n - 1]"
        />
      </div>

      <TodayDisplay
        v-if="show === false"
        :sol="this.sols[daysToDisplay - 1]"
        :tempMax="this.tempMax[daysToDisplay - 1]"
        :tempMin="this.tempMin[daysToDisplay - 1]"
        :windSpeed="this.windSpeed[daysToDisplay - 1]"
        :atmosP="this.atmosP[daysToDisplay - 1]"
        :date="this.dates[daysToDisplay - 1]"
      />
    </div>
    <div v-if="show === false" id="keyText">
      <p>SOL = Solar Day in the Martian Year |</p>
      <p>HIGH = Highest Recorded Temperature of the day in Fahrenheit |</p>
      <p>LOW = Lowest Recorded Temperature of the day in Fahrenheit |</p>
      <p>AWS = Average Windspeed for the day in metres per second |</p>
      <p>AAP = Average Atmospheric Pressure for the day in newtons</p>
    </div>
  </div>
</template>

<script>
import Title from "./components/Title.vue";
import DayDisplay from "./components/DayDisplay.vue";
import TodayDisplay from "./components/TodayDisplay.vue";

export default {
  name: "App",
  data() {
    return {
      show: false,
      bgURL: "",
      daysToDisplay: 0,
      sols: [],
      tempMax: [],
      tempMin: [],
      windSpeed: [],
      atmosP: [],
      dates: [],
    };
  },
  created() {
    console.log("This happened in created");
    this.hitAPIAPOD();
    this.hitAPIWeatherData();
  },
  methods: {
    changeShow: function (event) {
      this.show = !this.show;

      if (this.show === false) {
        document.body.background = "";
      } else {
        document.body.background = this.bgURL;
        document.body.style.backgroundSize = "contain";
        document.body.style.backgroundRepeat = "no-repeat";
        document.body.style.backgroundPosition = "center";
      }
    },
    hitAPIAPOD: function () {
      fetch(
        "https://api.nasa.gov/planetary/apod?api_key=zsjFBqwYnVhAMEllKlXlkfgic3ZCO1GQ8AH0f6H0"
      )
        .then((response) => response.json())
        .then((response) => (this.bgURL = response.url));
    },
    hitAPIWeatherData: function () {
      fetch(
        "https://api.nasa.gov/insight_weather/?api_key=zsjFBqwYnVhAMEllKlXlkfgic3ZCO1GQ8AH0f6H0&feedtype=json&ver=1.0"
      )
        .then((response) => response.json())
        .catch((e) => console.error(e.message))
        .then((data) => {
          this.daysToDisplay = data.sol_keys.length;

          for (var key in data.sol_keys) {
            //checks if they don't exist in the fetch

            var date = data[data.sol_keys[key]].Last_UTC;

            var numbers = date.substring(0, 10);
            var splits = numbers.split("-");
            var monthDigits = splits[1];
            var month = "";

            if (monthDigits === "01") {
              month = "Janurary";
            } else if (monthDigits === "02") {
              month = "February";
            } else if (monthDigits === "03") {
              month = "March";
            } else if (monthDigits === "04") {
              month = "April";
            } else if (monthDigits === "05") {
              month = "May";
            } else if (monthDigits === "06") {
              month = "June";
            } else if (monthDigits === "07") {
              month = "July";
            } else if (monthDigits === "08") {
              month = "August";
            } else if (monthDigits === "09") {
              month = "September";
            } else if (monthDigits === "10") {
              month = "October";
            } else if (monthDigits === "11") {
              month = "November";
            } else if (monthDigits === "12") {
              month = "December";
            }

            this.dates[key] = month + " " + splits[2];

            this.sols[key] = data.sol_keys[key];

            var test = JSON.stringify(data[data.sol_keys[key]]);
            if (!test.includes("PRE")) {
              this.atmosP[key] = "N/A";
            }

            if (!test.includes("AT")) {
              this.tempMax[key] = "N/A";
              this.tempMin[key] = "N/A";
            }

            if (!test.includes("HWS")) {
              this.windSpeed[key] = "N/A";
            }

            for (var prop in data[data.sol_keys[key]]) {
              //Finds the actual values
              if (data[data.sol_keys[key]].hasOwnProperty(prop)) {
                if (prop === prop.toUpperCase()) {
                  if (prop !== "WD") {
                    if (prop === "PRE") {
                      this.atmosP[key] = data[data.sol_keys[key]][prop].av;
                    }

                    if (prop === "AT") {
                      this.tempMax[key] = data[data.sol_keys[key]][prop].mx;

                      this.tempMin[key] = data[data.sol_keys[key]][prop].mn;
                    }

                    if (prop === "HWS") {
                      this.atmosP[key] = data[data.sol_keys[key]][prop].av;
                    }
                  }
                }
              }
            }
          }
        });
    },
  },
  components: {
    Title,
    DayDisplay,
    TodayDisplay,
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Aldrich");

:root {
  --shade: 0.6;
  --url: url("https://images.pexels.com/photos/41951/solar-system-emergence-spitzer-telescope-telescope-41951.jpeg?auto=compress&cs=tinysrgb&dpr=3&h=750&w=1260");
}

html {
  background-image: linear-gradient(
      to bottom,
      rgba(0, 0, 0, var(--shade)),
      rgba(0, 0, 0, var(--shade))
    ),
    var(--url);
  background-size: cover;
}

#apod {
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
  border: 0;
  outline: none;
  outline-style: none;
}

#app {
  font-family: "Aldrich", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#displayContainer {
  display: flex;
  flex-direction: row;
  margin-top: 80px;
  align-items: flex-end;
}

#keyText > * {
  font-size: 8px;
  color: rgba(255, 255, 255, 0.35);
  display: inline-block;
  margin-right: 3px;
}

.button {
  font-family: "Aldrich";
  margin-top: -50px;
  position: absolute;
  background-color: rgba(255, 69, 0, 0.6);
  border-radius: 50px;
  font-size: 12px;
  color: white;
  cursor: pointer;
  width: 160px;
  height: 30px;
  display: flex;
  justify-content: center;
  align-items: center;
  animation: hoverOff 0.4s;
}

.button:hover {
  height: 40px;
  width: 230px;
  font-size: 16px;
  animation: hoverOn 0.4s;
}

@keyframes hoverOn {
  0% {
    height: 30px;
    width: 160px;
    font-size: 12px;
  }
  100% {
    height: 40px;
    width: 230px;
    font-size: 16px;
  }
}

@keyframes hoverOff {
  0% {
    height: 40px;
    width: 230px;
    font-size: 16px;
  }
  100% {
    height: 30px;
    width: 160px;
    font-size: 12px;
  }
}
</style>
