<script>
  import axios from 'axios';

  export default {
    data(){
      return{
        city: "",
        error:"",
        info: null,
      }
    },
    computed:{
      cityName(){
        return "«" + this.city + "»"
      },
      showTemp(){
        return "Температура: " + this.info.main.temp  
      },
      showFeelsLike(){
        return "Ощущается как: " + this.info.main.feels_like  
      },
      showMinTemp(){
        return "Минимальная температура: " + this.info.main.temp_min  
      },
      showMaxTemp(){
        return "Максимальная температура: " + this.info.main.temp_max  
      },
    },
    methods: {
      getWeather(){
        if (this.city.trim().length < 2){
          this.error = "Нужно название более 1 символа"
          return false
        }
        
        this.error = ""

        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=3d9de74844d28377e81415151cbe6a66`)
          .then(res => (this.info = res.data))
      }
    }
  }
</script>

<template>
  <div class="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{ city == "" ? "вашем городе" : cityName }}</p>
    <input type="text" placeholder="Введите город" v-model="city">
    <button v-if="city != ''" @click="getWeather()">Получить данные о погоде</button>
    <button disabled v-else>Введите название города</button>
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
.error{
  color: red;
}
.wrapper{
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background: #1f0f24;
  padding: 20px;
  text-align: center;
  color: white;
}
.wrapper h1{
  margin-top: 50px;
}
.wrapper p {
  margin-top: 20px;
}
.wrapper input{
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  padding: 5px 8px;
  outline: none;
}
.wrapper input:focus{
  border-bottom-color:#6e2d7d;
}
.wrapper button{
  background: #104c4a;
  color: white;
  border-radius: 10px;
  border: 2px solid #1f9f73;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}
.wrapper button:disabled{
  background:grey;
  cursor: not-allowed;
  border: 2px solid rgb(59, 59, 59)
}
.wrapper button:hover{
  transform: scale(1.1) translateY(-5px);
}
</style>
