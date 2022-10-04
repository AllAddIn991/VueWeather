<script>
  import axios from 'axios'

  export default {
    data() {             // Объект
      return {
        city: "",    // Переменная
        error: "",  
        info: null,
      }
    },
    computed: {
      cityName() {
        return "<" + this.city + ">"
      },
      showTemp() {
        return "Температура: " + this.info.main.temp
      },
      feelLike() {
        return "Ощущается как: " + this.info.main.feels_like 
      },
      minTemp() {
        return "Минимальная температура: " + this.info.main.temp_min
      },
      maxTemp() {
        return "Максимальная Температура: " + this.info.main.temp_max
      },
    },
    methods: {
      getWeather() {
        if(this.city.trim().length < 2) {
          this.error = "Нужно написать название более 1 символа"
          return false
        }

        // 806855f3b69a4580f40049aef6037292 91da06d6a39c7935c0b009b8c059e16b
        this.error = ""
        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=91da06d6a39c7935c0b009b8c059e16b`)
        .then(res => (this.info=res.data))
      }
    }
  }
</script>

<template>

<div class="wrapper">
  <h1>Погодное приложение </h1>
  <p> Узнать погоду в {{ city == "" ? "Вашем городе" : cityName }} </p>
  <input type="text"  v-model="city" placeholder="Выберите город">
  <button v-if="city != ''" @click="getWeather()"> Узнать погоду</button>
  <button disabled v-else> Введите название города </button>
  <p class="error"> {{error}} </p>
    <div v-if = "info != null">
    <p > {{ showTemp }} </p>
    <p > {{ feelLike }} </p>
    <p > {{ minTemp }} </p>
    <p > {{ maxTemp }} </p>
    </div>
</div>
</template>

<style scoped>
.error {
  color: rgb(169, 26, 26);
}

  .wrapper {
    width: 900px;
    height: 500px;
    border-radius: 50px;
    background: rgb(41, 27, 111);
    padding: 20px;  
    text-align: center;
    color: aliceblue;
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
    border-bottom: 2px solid rgb(7, 4, 4);
    color: rgb(152, 216, 195);
    font-size: 14px;
    padding: 5px 8px;
    outline: none;
  }

  .wrapper input:focus {
    border-bottom-color: rgb(8, 21, 164);
  }
  .wrapper button:disabled {
    background: rgb(191, 217, 219);
    cursor:not-allowed;

  }

  .wrapper button {
    background: yellow;
    color: rgb(5, 12, 12);
    border-radius: 0px;
    border: 2px solid orange;
    padding: 10px 15px;
    margin-left: 20px;
    cursor: pointer;
    transition: transform 500ms ease;
  }

  .wrapper button:hover {
    transform: scale(1.1) translateY(-5px);
  }

</style>
