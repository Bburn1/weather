<template>
<div>
       <table align="center">
           <tr>
               <td ><input type="text" v-model="weathercityAdd">
                    <br>
                    <button class="myButton" v-on:click="addWeatherCity">Add</button>
                    </td>
                     <td> 
                    <select v-model="weathercity" style>
                        <option  v-for="city in weathercities" v-bind:key="city" v-bind:value="city">{{city}}</option>
                    </select>
                    <br><button class="myButton" v-on:click="getWeather">Weather</button>
                </td>
            </tr>
       </table>
       <table v-if="weather.cod >0" cellspacing="40px" align="center"> 
            <tr>
                <td>{{weathercity}}</td> 
                <td>{{weather.sys.country}}</td> 
            </tr>   

            <tr>
                <td>Sky</td> 
                <td>{{weather.weather[0].description}}</td>
            </tr>
            <tr>
                <td>Temp</td>
                <td>{{weather.main.temp-273.15 | roundValue}}°C</td>
            </tr>
            <tr>
                <td>Pressure</td>  
                <td>{{weather.main.pressure}} P</td>
            </tr>
            <tr>
                <td>Humidity</td> 
                <td >{{weather.main.humidity}} г/м³</td>
            </tr>
       </table>
   </div>
</template>

<script>
    
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'


    export default {
    data: function() {
        return {
        weathercities:[],
        weather:[],
        weathercityAdd:"",
        weathercity:"",
 
    }},
    mounted: function(){
         if(localStorage.getItem("weathercities") !== '') this.weathercities = JSON.parse(localStorage.getItem("weathercities")|| "[]");
    },
    methods:{

         getWeather: function() {
                 axios.get("https://api.openweathermap.org/data/2.5/weather?q="+this.weathercity+"&appid=7914d5a440960cfd5df3bd0388a7ad0f", {
                          units: "standart",
                })
                .then((response)=>{
                    console.log(response.data);
                    this.weather = response.data;
                    
                })
            },
            addWeatherCity: function(){
                this.weathercities.push(this.weathercityAdd)
                localStorage.setItem("weathercities",JSON.stringify(this.weathercities))
            },

    },
    filters:{
            roundValue: function(value){
                return parseFloat(value.toFixed(2));
            },
        }
    }
    
</script>
