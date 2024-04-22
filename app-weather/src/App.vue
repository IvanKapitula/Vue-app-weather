<script>
import axios from "axios";
export default {
  data() {
    return {
      city: "",
      error: "",
      info: null,
    };
  },
  computed: {
    cityName() {
      return "--" + this.city + "--";
    },
    showTemp() {
      return "Temperature: " + this.info.main.temp;
    },
    showFeelsLike() {
      return "Feels like: " + this.info.main.feels_like;
    },
    showMinTemp() {
      return "Minimum temperature: " + this.info.main.temp_min;
    },
    showMaxTemp() {
      return "Maximum temperature: " + this.info.main.temp_max;
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Required name is more than one character!";
        return false;
      }
      this.error = "";
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=3b351dc551043bd6dd2eab61b8e62437`
        )
        .then((res) => (this.info = res.data));
    },
  },
};
</script>

<template>
  <div class="wrapper">
    <h1>Weather app</h1>
    <p>
      Find out the weather in
      {{ city == "" ? "your city" : cityName }}
    </p>
    <input type="text" v-model="city" placeholder="Enter city" />
    <button v-if="city != ''" @click="getWeather()">Receive weather</button>
    <button disabled v-else>Enter the name of the city</button>
    <p class="error">{{ error }}</p>

    <div v-if="info != null">
      <p>{{ showTemp }}</p>
      <p>{{ showFeelsLike }}</p>
      <p>{{ showMinTemp }}</p>
      <p>{{ showMaxTemp }}</p>
    </div>
  </div>
</template>

<style scoped>
.error {
  color: rgb(178, 65, 65);
}
.wrapper {
  width: 90vw;
  height: 90vh;
  border-radius: 50px;
  padding: 20px;
  background: #9886c2;
  text-align: center;
  color: #fff;
  margin: 2vw;
}
.wrapper h1 {
  margin-top: 50px;
}
.wrapper p {
  margin-top: 20px;
}
.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid rgb(229, 231, 234);
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
  margin-bottom: 20px;
}
.wrapper input:focus {
  border-bottom-color: rgba(1, 1, 3, 0.251);
}
.wrapper button:disabled {
  background: rgb(91, 91, 181);
  cursor: not-allowed;
}
.wrapper button {
  background: blue;
  color: white;
  border-radius: 10px;
  border: 3px solid rgba(1, 1, 3, 0.251);
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}
</style>
