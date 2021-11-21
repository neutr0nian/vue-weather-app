<template>
    <div id="app" v-bind:class="isDay()?'bgDay':'bgNight'">
        <main>
            <div class="search-box">
                <input
                    type="text"
                    class="search-bar"
                    placeholder="Your City"
                    v-model="cityName"
                    v-on:keypress="getWeather"
                />
            </div>
            <div v-if="Object.keys(currentWeather).length > 0">
                <b-container>
                    <b-row>
                        <b-col>
                            <b-card
                                :title="cityName + ' ' + country"
                                class="card main-card text-center mx-auto"
                            >
                                <b-card-text class="text-lg">
                                    {{ Math.round(currentWeather.main.temp) }}&deg;C
                                </b-card-text>
                                <b-card-text class="text-sm">
                                  {{currentWeather.weather[0].description}}
                                </b-card-text>
                                <b-card-text class="text-sm">
                                    High: {{ Math.round(currentWeather.main.temp_max) }}&deg;
                                    Low: {{ Math.round(currentWeather.main.temp_min) }}&deg;
                                </b-card-text>
                            </b-card>
                        </b-col>
                    </b-row>
                </b-container>
                <b-container class="mt-4">
                    <b-row>
                        <b-col>
                            <b-card title="Weather Trends">
                                <b-row class="mt-4">
                                    <b-col>
                                        <b-card>
                                            <b-card-text>Wind</b-card-text>
                                            <b-row
                                                class="justify-content-center"
                                            >
                                                <b-icon
                                                    icon="compass"
                                                    class="mt-2 mb-4"
                                                    style="
                                                        width: 100px;
                                                        height: 100px;
                                                    "
                                                ></b-icon>
                                            </b-row>
                                            <b-card-text
                                                >Wind at: {{
                                                    currentWeather.wind["speed"]
                                                }}
                                                m/h
                                            </b-card-text>
                                        </b-card>
                                    </b-col>
                                    <b-col>
                                        <b-card>
                                            <b-card-text
                                                >Sunset:
                                                {{
                                                    getDate(currentWeather.sys.sunset, 'hour')
                                                }}:{{
                                                    getDate(currentWeather.sys.sunset, 'min')
                                                }}</b-card-text
                                            >
                                            <b-row
                                                class="justify-content-center"
                                            >
                                                <b-icon
                                                    icon="sun"
                                                    class="mt-2 mb-4"
                                                    style="
                                                        width: 100px;
                                                        height: 100px;
                                                    "
                                                ></b-icon>
                                            </b-row>
                                            <b-card-text
                                                >Sunrise:
                                                {{
                                                   getDate(currentWeather.sys.sunrise, 'hour')
                                                }}:{{
                                                    getDate(currentWeather.sys.sunrise, 'min')
                                                }}</b-card-text
                                            >
                                        </b-card>
                                    </b-col>
                                    <b-col>
                                        <b-card>
                                            <b-card-text
                                                >Feels like
                                            </b-card-text>
                                            <b-row
                                                class="justify-content-center"
                                            >
                                                <b-icon
                                                    icon="thermometer"
                                                    class="mt-2 mb-4"
                                                    style="
                                                        width: 100px;
                                                        height: 100px;
                                                    "
                                                ></b-icon>
                                            </b-row>
                                            <b-card-text>It feels like {{Math.round(currentWeather.main.feels_like)}}&deg;C </b-card-text>
                                        </b-card>
                                    </b-col>

                                    <b-col>
                                        <b-card>
                                            <b-card-text>Humidity</b-card-text>
                                            <b-row
                                                class="justify-content-center"
                                            >
                                                <b-icon
                                                    icon="droplet-half"
                                                    class="mt-2 mb-4"
                                                    style="
                                                        width: 100px;
                                                        height: 100px;
                                                    "
                                                ></b-icon>
                                            </b-row>
                                            <b-card-text
                                                >Dew point is
                                                {{currentWeather.main.humidity}}&deg;</b-card-text
                                            >
                                        </b-card>
                                    </b-col>
                                    <b-col>
                                        <b-card>
                                            <b-card-text
                                                >Visibility</b-card-text
                                            >
                                            <b-row
                                                class="justify-content-center"
                                            >
                                                <b-icon
                                                    icon="eye"
                                                    class="mt-2 mb-4"
                                                    style="
                                                        width: 100px;
                                                        height: 100px;
                                                    "
                                                ></b-icon>
                                            </b-row>
                                            <b-card-text
                                                >Visible {{Math.round(currentWeather.visibility/1609)}} miles</b-card-text
                                            >
                                        </b-card>
                                    </b-col>
                                    <b-col>
                                        <b-card>
                                            <b-card-text>Pressure</b-card-text>
                                            <b-row
                                                class="justify-content-center"
                                            >
                                                <b-icon
                                                    icon="arrows-collapse"
                                                    class="mt-2 mb-4"
                                                    style="
                                                        width: 100px;
                                                        height: 100px;
                                                    "
                                                ></b-icon>
                                            </b-row>
                                            <b-card-text
                                                >Pressure is {{Math.round(currentWeather.main.pressure)}}</b-card-text
                                            >
                                        </b-card>
                                    </b-col>
                                </b-row>
                            </b-card>
                        </b-col>
                    </b-row>
                </b-container>
                <b-container class="mt-4">
                    <b-row>
                        <b-col>
                            <b-card title=" 48 Hrs Forecast">
                                <b-table
                                    class="mt-4"
                                    sticky-header
                                    :fields="fields"
                                    :items="hourly"
                                    tbody-tr-class="text-white"
                                ></b-table>
                            </b-card>
                        </b-col>
                    </b-row>
                </b-container>
            </div>
        </main>
    </div>
</template>

<script>
export default {
    name: "App",
    components: {},
    data() {
        return {
            api1Base: "https://api.openweathermap.org/data/2.5/weather?",
            api2Base: "https://api.openweathermap.org/data/2.5/onecall?",
            apiKey: "71f2ca410cc6969b677874f2d1fe31e9",
            cityName: "",
            country: "",
            lat: 0,
            lon: 0,
            currentWeather: {},
            fields:[
              {
                key: 'day',
                thStyle:{
                  color: '#000000',
                  background:'#ffffff',
                  borderRadius:'10px 0px 0px 10px'
                }
              },
               {
                key: 'time',
                thStyle:{
                  color: '#000000',
                  background:'#ffffff',
                  borderRadius:'0px 0px 0px 0px'
                }
              },
              {
                key: 'temp',
                thStyle:{
                  color: '#000000',
                  background:'#ffffff'
                }
              },
              {
                key: 'feels like',
               thStyle:{
                  color: '#000000',
                  background:'#ffffff'
                }
              },
              {
                key: 'wind speed',
                thStyle:{
                  color: '#000000',
                  background:'#ffffff'
                }
              },
              {
                key: 'description',
                thStyle:{
                  color: '#000000',
                  background:'#ffffff',
                  borderRadius:'0px 10px 0px 0px'
                },
              }
            ],
            hourly: [],
            days: [
                "Sunday",
                "Monday",
                "Tuesday",
                "Wednesday",
                "Thursday",
                "Friday",
                "Saturday",
            ],
        };
    },
    methods: {
        getWeather(event) {
            if (event.key == "Enter") {
                fetch(
                    `${this.api1Base}q=${this.cityName}&units=metric&appid=${this.apiKey}`
                )
                    .then((response) => response.json())
                    .then((data) => {
                        this.lat = data.coord.lat;
                        this.lon = data.coord.lon;
                        this.cityName = data.name;
                        this.country = data.sys.country;
                        this.displayData(data);
                    });
            }
        },
        displayData(data) {
            fetch(
                `${this.api2Base}lat=${this.lat}&lon=${this.lon}&units=metric&appid=${this.apiKey}`
            )
                .then((response) => response.json())
                .then((data) => {
                    this.hourly = data.hourly.map((e) => {
                        return {
                            day: this.days[this.getDate(e.dt, 'day')],
                            time:this.getDate(e.dt, 'hour') + ':00' ,
                            temp: Math.round(e.temp),
                            "feels like": Math.round(e.feels_like),
                            "wind speed": e.wind_speed + " mph",
                            description: e.weather[0]["description"],
                        };
                    });
                });
            this.currentWeather = data;
        },
        getDate(timestamp, type){
          let date = new Date(timestamp*1000);
          switch(type){
            case 'day':
              return date.getDay();
            case 'min':
              return date.getMinutes();
            case 'hour':
              return date.getHours();
          }
        },
        isDay(){
          let date = Date.now();
          let hour = new Date(date).getHours();
          console.log(hour);
          if(hour < 16 && hour > 6){
            return true
          }
          return true
        }
    },
};
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
body {
    font-family: "montserrat", sans-serif;
}
#app {
    background-size: cover;
    /* background-color: #24baffa2; */
}
main {
    min-height: 100vh;
    padding: 25px;
}
.text-white{
  color: #fff;
}
.text-sm{
  font-size: 16px;
}
.text-lg{
  font-size: 42px!important;
}
.bgDay{
  background:linear-gradient(0deg, rgba(128, 128, 128, 0.281), rgba(128, 128, 128, 0.3)),
  url('../src/assets/sunny.jpeg');
  background-position: center;
}

.bgNight{
  background:linear-gradient(0deg, rgba(37, 37, 37, 0.616), rgba(37, 37, 37, 0.562)),
  url('../src/assets/night.jpg');
  background-position: center;
}
.search-box {
    width: 100%;
    margin-bottom: 30px;
}
.search-box .search-bar {
    display: block;
    width: 50%;
    padding: 15px;

    color: #313131;
    font-size: 20px;

    margin: 0 auto 0 auto;

    appearance: none;
    border: none;
    outline: none;
    background: none;

    box-shadow: 0px 0px 0px rgba(0, 0, 0, 0.25);
    background-color: rgb(255, 255, 255);
    border-radius: 16px 0px 16px 0px;
    transition: 0.4s;
}

.search-box .search-bar:focus {
    box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.808);
    border-radius: 0px 16px 0px 16px;
}

.card {
    color: #ffffff;
    border-radius: 32px;
    box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
    background-color: rgba(85, 85, 85, 0.336) !important;
  
}

.main-card {
    font-size: 32px;
    width: 40%;
    font-weight: 300;
}
</style>
