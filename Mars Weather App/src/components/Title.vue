<template>
  <div class="title">
    <h1>MARS WEATHER APP</h1>
    <h3 class="subtitle">
      Uses publicly available information from the NASA Open API
    </h3>
    <a class="subtitle" href="https://api.nasa.gov/">https://api.nasa.gov/</a>
  </div>
</template>

<script>
export default {
  name: "Title",
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css?family=Aldrich");

* {
  font-family: "Aldrich";
}

h1 {
  color: white;
  font-size: 50px;
}

.subtitle {
  color: white;
  font-size: 14px;
  margin-top: -20px;
  margin-bottom: 0px;
}
</style>
