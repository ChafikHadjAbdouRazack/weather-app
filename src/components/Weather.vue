<template>
    <div class="weather">
        <form v-on:submit.prevent="getWeather">
            <ion-grid>
              <ion-row color="primary" justify-content-center>
                <ion-col align-self-center size-md="6" size-lg="5" size-xs="12">
                  <div padding>
                      <ion-item>
                          <ion-input type="text" name="city" placeholder="Enter your city name"></ion-input>
                      </ion-item>
                  </div>
                   <ion-button color="primary" type="submit">Get it</ion-button>
                </ion-col>
              </ion-row>
            </ion-grid>
        </form>
       <ion-grid>
           <ion-row color="primary" justify-content-center>
              <ion-col align-self-center size-md="6" size-lg="5" size-xs="12">
                 <ion-card>
                     <ion-card-header>
                        <ion-card-title>{{ country && city ? city+', '+country : 'No country' }}</ion-card-title>
                     </ion-card-header>
                     <ion-item lines="none" text-center>
                        <ion-label>{{ weatherDescription ? weatherDescription : '' }}</ion-label>
                     </ion-item>
                     <ion-item style="display: block!important; margin: 0 auto!important;" lines="none">
                        <ion-img  :src="icon" v-if="icon" alt="Weather Icon"></ion-img>
                        <ion-label>{{ temp ? temp+'°C' : ''}}</ion-label>
                     </ion-item>
                     <ion-item line="none">
                        <ion-label>{{ tempMin ? tempMin+'°C' : '' }}</ion-label>
                        <ion-label>{{ tempMax ? tempMax+'°C' : '' }}</ion-label>
                        <ion-label>{{ humidity ? 'Humidity: '+humidity+'%' : '' }}</ion-label>
                     </ion-item>
                 </ion-card>
              </ion-col>
           </ion-row>
       </ion-grid>
    </div>
</template>

<script>
    import axios from "axios"
    export default {
        name: "Weather",
        data(){
            return {
                city: '',
                country:'',
                icon:'',
                weatherDescription:'',
                temp:'',
                tempMin:'',
                tempMax:'',
                humidity:'',
                API_KEY:'d63783958914771e7b1abff1566d9001'
            }
        },
        methods:{
            getWeather(event){
                this.city=event.target[0].value
                axios.get('https://api.openweathermap.org/data/2.5/weather?q='+this.city+'&mode=json&units=metric&appid='+this.API_KEY).then(response=>{
                    console.log(response.data)
                    let data =response.data
                    this.country = data.sys.country
                    this.temp = data.main.temp
                    this.tempMin = data.main.temp_min
                    this.tempMax = data.main.temp_max
                    this.humidity = data.main.humidity
                    this.weatherDescription = data.weather[0].description
                    this.icon = 'https://openweathermap.org/img/w/'+data.weather[0].icon+'.png'
                })
            }
        }
    }
</script>

<style scoped>

</style>