<template>
  <div id="app">
    <div class="main" v-bind:class="{warm: data.state_weather}" >
<div class="search-box">
        <input class="search-bar" type="text" placeholder="search......" v-model="data.city" 
                @keyup.enter="getApi()" >
       <div v-if="data.weather">
       <div class="header">
           <h1> {{data.weather.name}} </h1>
           <h3> {{new Date().toLocaleString()}} </h3>
      </div>
       <div class="temp">
           <h2>{{Math.round(data.weather.main.temp)}}&deg;</h2>
      </div>
      <div class="state">
           <h3>{{ data.weather.weather[0].main }} </h3>
      </div>
      </div>
      </div>
      </div>
    </div>  
</template>

<script>
import axios from 'axios'
export default {
  name: 'WeatherApp',
  props: { 
  },
  data(){
    return{
      data : {
        weather:null,
        city:'',
        api_key:'6077fd7b0d301e8bd0fccf10d3b3c748',
        current_day:'',
        state_weather:false
    }
    }
  },
  mounted:async function(){
    this.getApi()
  },

  methods:{
     async getApi(){
      if(this.data.city===''){
        this.data.city='Hubli'
      }
      const getWeather = await axios.get( 
        `https://api.openweathermap.org/data/2.5/weather?units=metric&q=${this.data.city}&appid=${this.data.api_key}`
      )
      this.data.weather = getWeather.data
      this.data.city=''
      this.data.state_weather = false
      if(this.data.weather.main.temp > 20){
        this.data.state_weather = true
      }else{
        this.data.state_weather = false
      }
      console.log(this.data.weather)
    }
  }
}
</script>

<style scoped>
*{
  margin:0;
  padding:0;
  box-sizing: border-box;
}

#app{
  background-image: url('https://img.freepik.com/free-vector/gorgeous-clouds-background-with-blue-sky-design_1017-25501.jpg');
  background-size: cover;
  background-position: bottom;
  /* transition: 0.4s;  */
}
.main{
  min-height: 100vh;
  padding:25px;
  background-image: linear-gradient(
    to baottom,
    rgba(18,75,156,0.35),
    rgba(2,15,22,0.75));

}
.main.warm{
  background-image: linear-gradient(
    to bottom,
    rgba(212, 69, 69, 0.35),
    rgba(246, 6, 6, 0.75)
  );
  min-height: 100vh;
  padding:25px;
}
.search-box{
  width:100%;
  left:100px;
  margin:200px;
  position:relative;
}
.search-box .search-bar{
  display: block;
  width:20%;
  padding:15px;
  color:#313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background-color: rgba(255,255,255,0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px;
  background-color: rgba(255,255,255,0.75);
}
.header{
  padding-top: 20px;
  font-size: 20px;
  color:rgb(37, 43, 43);
  box-shadow: rgba(3,3,3,0.3); 

}
.temp{
  display: inline-block;
  padding:10px 25px;
  color:#FFF;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0,0,0,0.25);


  background-color: rgba(255,255,255,0.25);
  border-radius: 16px;
  margin:30px 0px;
  box-shadow: 3px 6px rgba(0,0,0,0.25);
}
.state{
  position: absolute;
  color:#FFF;
  font-size: 48px;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  text-align: center;
}

</style>

