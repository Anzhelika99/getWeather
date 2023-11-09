<template>
  <div class="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{ city ? cityName : "Вашем городе" }}</p>
    <input type="text" v-model="city" placeholder="Введите город" />
    <button v-if="city != ''" @click="getWeather()">Получить погоду</button>
    <button disabled v-else>Введите название города</button>
    <p class="error">{{ error }}</p>
    <div class="show-temp" v-if="info != null">
      <p>{{ showTemp }}</p>
      <p>{{ showFeelsLike }}</p>
      <p>{{ showMinTemp }}</p>
      <p>{{ showMaxTemp }}</p>
    </div>
  </div>
</template>

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
      return `"${this.city}"`;
    },
    showTemp() {
      return `Температура: ${this.info.temp}°C`;
    },
    showFeelsLike() {
      return `Ощущается как: ${this.info.feels_like}°C`;
    },
    showMinTemp() {
      return `Минимальная температура: ${this.info.temp_min}°C`;
    },
    showMaxTemp() {
      return `Максимальная температура: ${this.info.temp_max}°C`;
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Нужно название более 1 символа :)";
        return false;
      }

      this.error = "";

      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=98b58d80fa8f2e05a327af2b52ae24a5`
        )
        .then((res) => (this.info = res.data.main))
        .catch((err) => alert(`Введите ${err}`));
    },
  },
};
</script>

<style lang="scss">
.error {
  color: #d03939;
}

.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background: #1f0f24;
  text-align: center;
  color: #fff;

  h1 {
    margin-top: 50px;
  }

  p {
    margin-top: 20px;
  }

  input {
    margin-top: 30px;
    background: transparent;
    border: 0;
    border-bottom: 2px solid #110813;
    color: #fefefe;
    font-size: 14px;
    padding: 5px 8px;
    outline: none;

    &:focus {
      border-bottom-color: #6e2d7d;
    }
  }

  input:focus {
    border-bottom-color: #6e2d7d;
  }

  button {
    background: #e3bc4b;
    border: 0;
    color: #ffffff;
    border-radius: 10px;
    padding: 10px 15px;
    margin-left: 20px;
    cursor: pointer;
    transition: transform 500ms ease;

    &:hover {
      transform: scale(1.1) translateY(-5px);
    }
    &:disabled {
      background: #746027;
      cursor: not-allowed;
    }
  }
}
</style>
