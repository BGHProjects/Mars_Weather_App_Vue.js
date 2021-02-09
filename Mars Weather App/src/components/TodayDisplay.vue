<template>
  <div class="displayCard">
    <h2>Sol {{ dsol }}</h2>
    <h2>{{ ddate }}</h2>

    <h3>High: {{ dtempMax }}</h3>
    <h3>Low: {{ dtempMin }}</h3>
    <h3>AWS: {{ dwindSpeed }}</h3>
    <h3>AAP: {{ datmosP }}</h3>
  </div>
</template>

<script>
export default {
  name: "TodayDisplay",
  props: {
    sol: {
      type: String,
    },
    tempMin: {
      type: String,
    },
    tempMax: {
      type: String,
    },
    windSpeed: {
      type: String,
    },
    atmosP: {
      type: String,
    },
    date: {
      type: String,
    },
  },
  data() {
    return {
      dsol: this.sol,
      dtempMax: this.tempMax,
      dtempMin: this.tempMin,
      dwindSpeed: this.windSpeed,
      datmosP: this.atmosP,
      ddate: this.date,
    };
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css?family=Aldrich");

* {
  font-family: "Aldrich";
  color: white;
}

.displayCard {
  border: 5px solid rgba(255, 69, 0, 0.6);
  border-radius: 50px;
  height: 300px;
  width: 200px;
  margin-left: 10px;
  margin-right: 10px;
  animation: hoverOff 0.4s;
}

.displayCard:hover {
  background-color: rgba(255, 69, 0, 0.6);
  border: 5px solid rgba(255, 69, 0, 0);
  animation: hoverOn 0.4s;
}

@keyframes hoverOn {
  0% {
    background-color: rgba(255, 69, 0, 0);
    border: 5px solid rgba(255, 69, 0, 0.6);
  }

  100% {
    background-color: rgba(255, 69, 0, 0.6);
    border: 5px solid rgba(255, 69, 0, 0);
  }
}

@keyframes hoverOff {
  0% {
    background-color: rgba(255, 69, 0, 0.6);
    border: 5px solid rgba(255, 69, 0, 0);
  }

  100% {
    background-color: rgba(255, 69, 0, 0);
    border: 5px solid rgba(255, 69, 0, 0.6);
  }
}

h2 {
  font-size: 25px;
}

h3 {
  font-size: 20px;
  margin-top: 3px;
  margin-bottom: 3px;
}
</style>