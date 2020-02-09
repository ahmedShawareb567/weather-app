<template>
  <div>
    <section class="weatherSec">
      <div class="container">
        <div class="row">
          <div class="col-12">
            <div class="weather-content">
              <h2>{{ weatherInfo.sys.country }}</h2>
              <span>{{ weatherInfo.name }}</span>
              <span>{{ weatherInfo.weather[0].main }}</span>
              <span>{{ weatherInfo.weather[0].description }}</span>
              <span class="temp">
                {{ weatherInfo.main.temp }} C
                <sup>0</sup>
              </span>
              <span>{{ currenDate }}</span>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>
<script>
export default {
  created() {
    this.getDimFunc();
    this.getDate();
  },
  mounted() {
    this.getWeather();
  },
  updated() {
    this.getWeather();
  },
  data() {
    return {
      dimensions: {
        x: "null",
        y: "null"
      },
      appApi: "1ffa5dd0114ddf20698a42bb5f66e6d2",
      base_url: "https://api.openweathermap.org/data/2.5/",
      weatherInfo: "",
      currenDate: "null"
    };
  },
  methods: {
    getDimFunc() {
      navigator.geolocation.getCurrentPosition(position => {
        this.dimensions.x = position.coords.latitude;
        this.dimensions.y = position.coords.longitude;
      });
    },
    getWeather() {
      let xDim, yDim;
      xDim = this.dimensions.x;
      yDim = this.dimensions.y;
      fetch(
        `${this.base_url}weather?lat=${Number.parseFloat(xDim).toFixed(
          2
        )}&lon=${Number.parseFloat(yDim).toFixed(2)}&units=metric&APPID=${
          this.appApi
        }`
      )
        .then(render => {
          return render.json();
        })
        .then(response => {
          this.weatherInfo = response;
        });
    },
    getDate() {
      setInterval(() => {
        let theDate = new Date(),
          months = [
            "January",
            "February",
            "March",
            "April",
            "May",
            "June",
            "July",
            "August",
            "September",
            "October",
            "November",
            "December"
          ],
          days = [
            "Sunday",
            "Monday",
            "Tuesday",
            "Wednesday",
            "Thursday",
            "Friday",
            "Saturday"
          ],
          year = theDate.getFullYear(),
          day = days[theDate.getDay()],
          month = months[theDate.getMonth()],
          hour = theDate.getHours() % 12,
          currentHour = hour < 10 ? `0${hour}` : hour,
          minutes =
            theDate.getMinutes() < 10
              ? `0${theDate.getMinutes()}`
              : theDate.getMinutes(),
          amPm = theDate.getHours() > 12 ? "PM" : "AM",
          seconds =
            theDate.getSeconds() < 10
              ? `0${theDate.getSeconds()}`
              : theDate.getSeconds();
        this.currenDate = `${day} ${year} ${month} ${currentHour} : ${minutes} : ${seconds} ${amPm.toUpperCase()}`;
      }, 1000);
    }
  }
};
</script>
