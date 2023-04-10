<template>
  <div class="container">
    <!-- <h1>vue weather</h1> -->
    <form class="search-box" action="" @submit.prevent="getWeather">
      <input type="text" v-model="query" placeholder="Search...">
      <div class="error"> {{ error }}</div>
    </form>
    <div v-if="typeof weather.main !='undefined'">
      <div class="location-box">
        <div class="city-name">{{weather.name}},{{ weather.sys.country }}</div>
        <div class="date">{{ getDate() }}</div>
        <div class="coord">Latitude : {{weather.coord.lat}}, Longitude : {{ weather.coord.lon }}</div>
      </div>
      <div class="weather-box">
        <div class="temp"> {{Math.round(weather.main.temp) }}℃</div>
        <div class="icon"><img :src="`https://openweathermap.org/img/wn/${weather.weather[0].icon}@2x.png`" alt=""></div>
        <div class="weather">{{weather.weather[0].main}}</div>
      </div>
    </div>
  </div>

</template>

<script>
import { ref } from 'vue'

export default {
  setup(){
    let weather = ref({})
    let query = ref('')
    let error = ref('')

    function getWeather(){
      if(query.value === ""){
        error.value = 'Please enter your city'
      }
      else{
        error.value = ''
      }
      fetch(`https://api.openweathermap.org/data/2.5/weather?q=${query.value}&units=metric&APPID=e250090c7ee7886adc78d7c0620d5d1f`)
        .then(res => {return res.json()})
        .then(data =>{
          weather.value = data
          // console.log(weather.value);
        })
        
    }
    // getApi()

    // function getDate(){
    //   today.value = weekday.value[date.getDay()]
    //   console.log(today.value);
    // }
   

    function getDate(){
    let d = new Date()
    let date = ref('')
    let day = ref('')
    let month = ref('')
    let year = ref('')
    const days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
    const months =["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"]
    
    date.value = d.getDate()
    day.value = days[d.getDay()]
    month.value = months[d.getMonth()]
    year.value = d.getFullYear()
    
    // console.log(d);
    // console.log('date is ' + date.value);
    // console.log('day is ' + day.value);
    // console.log('month is ' + month.value);
    // console.log('year is ' + year.value);

    return` ${day.value} ${date.value} ${month.value}  ${year.value}`
}
 getDate()

    return{getWeather, query ,weather,error,getDate}
  }
}
</script>

<style>
*{
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body{
  /* background: #c68e8e; */
  /* background: url('./assets/weather2.png') no-repeat center center/cover;
  height: 100vh; */
  /* position: relative;
  z-index: -2; */
}

body::after{
  /* content: '';
  background-color: #33333380;
  position: absolute;
  top: 0px;
  left: 0px;
  right: 0;
  width: 100vw;
  height: 100vh;
  z-index: -1; */
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 60px; */
  background: url('./assets/weather2.png') no-repeat center center/cover;
  background-position: bottom;
}
.container{
  height: 100vh;
  background: rgba(0, 0, 0, 0.5);
}
.search-box{
  padding-top: 60px;
}
.search-box input{
  appearance: none;
  width: 50%;
  /* height: 40px; */
  border-radius: 0 15px 0 15px;
  border:none;
  outline: none;
  background: none;
  padding: 15px;
  box-shadow:0px 0px 8px rgba(0, 0, 0, 0.25) ;
  background-color: rgba(255, 255, 255, 0.5);
  color: #313131;
  font-size: 15px;
  transition: 0.5s;
}
.search-box input:focus{
  border-radius: 15px 0px 15px 0px;
  box-shadow:0px 0px 10px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.75) ;

}

.error{
  color: rgb(255, 112, 112);
  font-size: 20px;
  /* text-align: left; */
  padding-top: 15px;
  font-weight: 700;
  font-style: italic;
}

.location-box .city-name{
  color: #fff;
  font-size: 30px;
  font-weight: 500;
  padding-top: 30px ;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date , .coord{
  font-size: 20px;
  color: #ffffffcd;
  font-weight: 300;
  font-style: italic;
  padding-top: 5px;
  padding-bottom: 10px;
}
.location-box .coord{
  color: #ffffff80 !important ;
}

.weather-box .temp{
  display: inline-block;
  background-color:rgba(255, 255, 255, 0.25)  ;
  color:#fff ;
  border-radius:15px ;
  box-shadow: 5px 5px 8px rgba(0, 0, 0, 0.5) ;
  font-size:100px ;
  font-weight: 900;
  padding: 10px 25px;
  text-shadow:1px 3px rgba(0, 0, 0, 0.25);
  margin: 30px 0;
}
.weather-box .weather{
  color:#fff ;
  font-size:40px ;
  font-weight: 700;
  font-style: italic;
  text-shadow:1px 3px rgba(0, 0, 0, 0.25);
}

</style>
