<template>
  <div id="app">
    <div class="widget">
      <!--      <section class="main_section" :style="[city.temp <15 ? {'background': 'url(' + winter + ') center center/cover no-repeat'} : {'background': 'url(' + summer + ') center center/cover no-repeat'}  ]">-->
      <section class="main_section" :style="{'background': 'url(' + bg_img + ') center center/cover no-repeat'}">

        <h1>Vue Weather</h1>
        <div class="top">
          <h2 v-show="weather.length===0 ">Plan your day ahead.</h2>
          <div class="name" v-show="weather.length>0 ">
            <p class="city_name"> {{ city.name }}</p>
            <p class="country">{{ city.country }}</p>
            <p class="date">{{ moment(city.curr_date).format("MMM Do YY") }}</p>
          </div>
        </div>
        <div class="bottom" v-show="weather.length>0">
          <div class="temp"><p>{{ city.temp }}</p></div>
          <div class="feels_like"><p>Feels Like <br>
            {{ city.feels_like }}</p></div>
          <div class="description">
            {{ city.description }}

          </div>
        </div>
      </section>

      <section id="info_section">
        <div class="search">
          <input type="text" placeholder="Search City" id="user_search" v-model="user_search">
          <img src="https://img.icons8.com/fluent-systems-filled/22/000000/search.png" @click="getWeather"/>
        </div>
        <div class="weather_details" v-show="weather.length>0">
          <weather :imgs="wind_icon" details1="Wind Speed" :details2="city.wind_speed"/>
          <weather :imgs="pressure_icon" details1="Pressure" :details2="city.pressure"/>
          <weather :imgs="humidity_icon" details1="Humidity" :details2="city.humidity"/>
        </div>
        <div class="forecast" v-show="weather.length>0">
          <h4> 3 days forecast</h4>
          <forecast :icon="forecast_data1.icon" :day="forecast_data1.forecast_day"
                    :date="forecast_data1.forecast_day"
                    :condition="forecast_data1.condition"
                    :max_temp="forecast_data1.max_temp"
                    :min_temp="forecast_data1.min_temp"/>


          <forecast :icon="forecast_data2.icon" :day="forecast_data2.forecast_day"
                    :date="forecast_data2.forecast_day"
                    :condition="forecast_data2.condition"
                    :max_temp="forecast_data2.max_temp"
                    :min_temp="forecast_data2.min_temp"/>

          <forecast :icon="forecast_data3.icon" :day="forecast_data3.forecast_day"
                    :date="forecast_data3.forecast_day"
                    :condition="forecast_data3.condition"
                    :max_temp="forecast_data3.max_temp"
                    :min_temp="forecast_data3.min_temp"/>
        </div>

      </section>
    </div>
  </div>
</template>


<script>
import weather from "@/components/weatherCard";
import forecast from "@/components/forecast";

export default {
  components: {
    weather,
    forecast
  },
  name: "App",
  data() {
    return {
      wind_icon: "https://img.icons8.com/cotton/34/ffffff/rain--v3.png",
      humidity_icon: "https://img.icons8.com/ultraviolet/34/000000/humidity.png",
      pressure_icon: "https://img.icons8.com/ultraviolet/40/000000/pressure.png",

      bg_img: require('./assets/home.jpg'),
      winter: require('./assets/snow.jpg'),
      summer: require('./assets/summer.jpg'),
      user_search: '',
      url: '',
      data: '',
      weather: [],
      /**
       * City: Object to hold data of interest from the payload recieved.
       */
      city: {
        name: "",
        feels_like: "",
        humidity: "",
        curr_date: "",
        temp: "",
        wind_dir: "",
        wind_degree: "",
        wind_speed: "",
        region: "",
        current_date: "",
        description: "",
        country: "",
        pressure: "",
      },
      forecast_data1: {
        condition: '',
        max_temp: '',
        min_temp: '',
        forecast_day: '',
        full_icon: '',
        icon: '',


      },
      forecast_data2: {
        condition: '',
        max_temp: '',
        min_temp: '',
        forecast_day: '',
        full_icon: "",
        short_icon: "",
        icon: '',

      },
      forecast_data3: {
        condition: '',
        max_temp: '',
        min_temp: '',
        forecast_day: '',
        icon: '',


      },

    }
  },
  methods: {
    /**
     *  getWeather: Async function communicate with API endpoint and extract data of interest. Returns object
     * @returns {Promise<string>}
     *
     *
     */

    getWeather: async function () {
      if (this.user_search !== "") {
        // this.url = 'https://api.codetabs.com/v1/proxy?quest=http://api.openweathermap.org/data/2.5/weather?q=' + this.user_search + '&appid=e9d9bf1bcc413384014325aa860c6171&units=metric'
        this.url = 'https://api.weatherapi.com/v1/forecast.json?key=42f8f2699bb64f0c8cb233647211902&q=' + this.user_search + '&days=3'

        this.resp = await fetch(this.url);
        this.data = await this.resp.json();
        //priting payload
        console.log(this.data);
        // console.log(this.data.current.condition.text);
        this.city.name = this.data.location.name;
        this.city.feels_like = this.data.current.feelslike_c;
        this.city.humidity = this.data.current.humidity;
        this.city.curr_date = this.data.location.localtime;
        this.city.temp = this.data.current.temp_c;
        this.city.wind_speed = this.data.current.wind_kph;
        this.city.wind_dir = this.data.current.wind_dir;
        this.city.wind_degree = this.data.current.wind_degree;
        this.city.region = this.data.location.region;
        this.city.country = this.data.location.country;
        this.city.description = this.data.current.condition.text;
        this.city.pressure = this.data.current.pressure_in;


        this.weather.push(this.city);

        this.forecast_data1.forecast_day = this.data.forecast.forecastday[0].date;
        this.forecast_data1.condition = this.data.forecast.forecastday[0].day.condition.text;
        this.forecast_data1.max_temp = this.data.forecast.forecastday[0].day.maxtemp_c;
        this.forecast_data1.min_temp = this.data.forecast.forecastday[0].day.maxtemp_c;
        let img_url1 = this.data.forecast.forecastday[0].day.condition.icon;
        let trimmed_url1 = img_url1.substr(img_url1.length - 12);
        this.forecast_data1.icon = require("./assets/Images" + trimmed_url1);

        this.forecast_data2.forecast_day = this.data.forecast.forecastday[1].date;
        this.forecast_data2.condition = this.data.forecast.forecastday[1].day.condition.text;
        this.forecast_data2.max_temp = this.data.forecast.forecastday[1].day.maxtemp_c;
        this.forecast_data2.min_temp = this.data.forecast.forecastday[1].day.maxtemp_c;
        let img_url2 = this.data.forecast.forecastday[1].day.condition.icon;
        let trimmed_url2 = img_url2.substr(img_url1.length - 12);
        this.forecast_data2.icon = require("./assets/Images" + trimmed_url2);

        console.log(typeof (this.city.temp));

        this.forecast_data3.forecast_day = this.data.forecast.forecastday[2].date;
        this.forecast_data3.condition = this.data.forecast.forecastday[2].day.condition.text;
        this.forecast_data3.max_temp = this.data.forecast.forecastday[2].day.maxtemp_c;
        this.forecast_data3.min_temp = this.data.forecast.forecastday[2].day.maxtemp_c;
        let img_url3 = this.data.forecast.forecastday[1].day.condition.icon;
        let trimmed_url3 = img_url3.substr(img_url3.length - 12);
        this.forecast_data3.icon = require("./assets/Images" + trimmed_url3);

        this.user_search = "";


      } else {
        alert("Please enter a city")
      }
      this.changeBG();
      return this.data;
    },
    /**
     * getDate: function that gets current data and time.
     */
  changeBG(){
    if(this.city.temp<15){
      this.bg_img = this.winter
    }else{
      this.bg_img= this.summer;
    }
    }

  },

  computed: {
    /**
     * getFlag: function that dynamically switch flags based on the country
     * @returns {string}
     */

  }
}
</script>


<style lang="scss">
@import './assets/Colors/colors';
@import './assets/Font/Font';

* {
  padding: 0;
  margin: 0;
  font-family: $text_font;
}

#app {
  margin: 0 auto;
  text-align: center;
  align-items: center;
  justify-content: center;

  .widget {
    border: 1px solid $background_color;
    display: flex;
    margin: 10vh auto;
    text-align: center;
    width: 60%;
    background: $background_color;
    height: 85vh;

    .main_section {
      //background: url('./assets/home.jpg') no-repeat center;
      background-size: cover;
      grid-column-start: 1;
      grid-column-end: 2;
      width: 60%;
      position: relative;
      height: 85vh;

      h1 {
        color: white;
        padding: 10px;
        text-align: start;
        font-weight: normal;
      }

      .top {

        .name {
          //color: $text_color;
          font-weight: bold;

          .city_name {
            margin: 0;
            padding: 0;
            font-weight: bolder;
            font-size: 60px;
            width: 100%;
            text-shadow: 0px 2px 2px rgb(35, 194, 137);
          }

          color: $text_color;

          .country {
            font-size: 16px;
            text-shadow: none;

          }

        }

        h2 {
          margin-top: 2vh;
          color: $text_color;
          opacity: 0.7;
          font-weight: normal;
        }
      }


      .bottom {
        position: absolute;
        bottom: 0;
        display: flex;
        width: 100%;
        justify-content: space-between;
        background-color: rgb(0, 0, 0); /* Fallback color */
        background-color: rgba(0, 0, 0, 0.4);

        .temp, .feels_like, .description {
          color: $text_color;
          font-size: 20px;
          padding: 10px;
          width: 100%;
          font-weight: bolder;

        }

        .temp {
          p {
            font-weight: bolder;
            color: $text_color;
            font-size: 120px;
            padding: 0;
            margin: 0;
          }
        }


      }

      .feels_like {
        display: block;
      }


    }

    #info_section {
      width: 40%;
      height: 90vh;
      position: relative;

      .search {
        position: relative;
        margin: 2vh auto;

        img {
          text-decoration: none;
          text-align: center;
          text-transform: uppercase;
          color: $text_color;
          background: none;
          letter-spacing: 2px;
          padding: 10px;
          cursor: pointer;
          font-weight: bolder;
          outline: none;
          background: $text_color;
          border-radius: 50%;
          box-shadow: rgba(50, 50, 93, 0.25) 0px 50px 100px -20px, rgba(0, 0, 0, 0.3) 0px 30px 60px -30px, rgba(10, 37, 64, 0.35) 0px -2px 6px 0px inset;
          position: relative;
          border: 1px solid $text_color;
        }

        input[type="text"] {
          transform: scale(0.8);
          position: relative;
          padding: 15px 20px 20px 15px;
          width: 120px;
          color: $text_color;
          font-size: 16px;
          font-weight: 100;
          letter-spacing: 2px;
          font-family: $text_font;
          transition: width 0.8s ease;
          outline: none;
          background: none;

          border: none;
          border-bottom: 2px solid $text_color;
          margin-top: 5px;

          &:focus {
            width: 220px;
            transform: scale(0.9);

          }

        }

        img {
          top: 10px;
          position: absolute;
          cursor: pointer;

        }
      }

      .weather_details {
        margin: 4vh auto;
        //background: $text_color;
      }

      .forecast {
        width: 100%;
        position: absolute;
        bottom: 60px;

        h4 {
          text-align: start;
          color: $highlight_color;
          padding: 10px;
        }
      }
    }

  }

  .winter {
    background-size: cover;
    background: url('./assets/snow.jpg') no-repeat center;
  }

  .summer {
    background-size: cover;
    background: url('./assets/summer.jpg') no-repeat center;
  }
}

</style>
