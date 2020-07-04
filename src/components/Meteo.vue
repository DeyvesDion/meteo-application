<template>
 <div class="container">
  <h1 class="title">Météo universelle</h1>
  <h2 class="subTitle">Rentrer une ville dans la barre de recherche</h2>
  <div class="box">
   <div class="search-box">
    <div class="form-group">
     <input
      type="text"
      class="search-bar form-control"
      id="location"
      placeholder="Exemple: Paris"
      v-model="query_rec"
      v-on:keypress="goMeteo"
     />
    </div>
   </div>
   <div class="weather-wrap" v-if="temp">
    <h2 class="location">{{ temp.name }}, {{ temp.sys.country }}</h2>
    <div class="weather-box">
     <h2 class="temp">{{ temp.main.temp.toFixed() }}<span>°C</span></h2>
     <div class="weather">
      <h2>{{ temp.weather[0].description }}</h2>
     </div>
    </div>
   </div>
  </div>
 </div>
</template>

<script>
import axios from "axios";
export default {
 name: "Meteo",
 data() {
  return {
   query_rec: "",
   temp: undefined,
   units_metric: "metric",
   api_key: "",
   url_query: "https://api.openweathermap.org/data/2.5/weather?",
  };
 },
 methods: {
  goMeteo(e) {
   if (e.key == "Enter") {
    axios
     .get(
      `
                      ${this.url_query}q=${this.query_rec}&APPID=${this.api_key}&units=${this.units_metric}&lang=fr`
     )
     .then((reponse) => {
      // console.log(reponse);
      this.temp = reponse.data;
      console.log(this.temp);
     })

     .catch((error) => {
      console.log(error.response);
     });
    this.query_rec = "";
   }
  },
 },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
* {
 font-family: "Montserrat", "sans-serif";
}
.box {
 height: auto;
 width: 700px;
 display: block;
 margin-top: 50px;
 margin-left: auto;
 margin-right: auto;
 background-color: #004643;
 border: solid 3px #d90368;
}
.title {
 text-align: center;
 text-transform: uppercase;
 font-size: 60px;
 color: #004643;
}
.subTitle {
 text-align: center;
 font-size: 20px;
 color: #004643;
}
.search-bar {
 border: solid 2px #d90368;
 height: 50px;
 width: 400px;
 border-radius: 1px 1px 1px 1px;
}

.search-box {
 display: flex;
 align-items: center;
 justify-content: center;
 height: 10px;
 margin-top: 50px;
 margin-bottom: 50px;
}

.location {
 color: #ffffff;
 text-align: center;
 text-transform: capitalize;
 font-size: 50px;
}
.weather-wrap {
 display: grid;
 align-items: center;
 justify-content: center;
 height: 50vh;
}
.weather-box {
 text-align: center;
 border: 1px solid #ffffff;
 border-radius: 1px 1px 1px 1px;
 outline: none;
 width: 400px;
 height: 150px;
 padding-top: 10px;
 box-shadow: 3px 1px 1px #edf2f4;
}
.temp {
 color: #ffffff;
 font-size: 55px;
}
.weather h2 {
 color: #ffffff;
 text-transform: capitalize;
 font-style: italic;
}

@media only screen and (max-width: 770px) {
 .title {
  font-size: 40px;
 }
 .box {
  width: auto;
 }
 .search-box {
  width: auto;
 }

 .search-bar {
  width: auto;
 }

 .weather-wrap {
  width: auto;
 }
}
</style>
