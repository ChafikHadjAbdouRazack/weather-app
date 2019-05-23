<template>
    <div class="weather">
        <form v-on:submit.prevent="getWeather">
            <ion-grid style="background: transparent;">
              <ion-row style="background: transparent; margin-top: 66px;" color="primary" justify-content-center>
                <ion-col style="background: transparent;" align-self-center size-md="6" size-lg="5" size-xs="12">
                  <div padding style="background: transparent;">
                      <ion-item style="--background: transparent;">
                          <ion-input class="item-highlight item-inner-highlight" style="--background: transparent; color: white; font-weight: bolder; font-size: larger;" @ionChange="getCity" type="text" name="city" placeholder="Enter your city name"></ion-input>
                      </ion-item>
                  </div>
                   <ion-button style="background: transparent; font-weight: bolder;" @click="presentLoading" v-bind:disabled="isDisabled" color="success" type="submit">Get it</ion-button>
                </ion-col >
              </ion-row>
            </ion-grid>
        </form>
       <ion-grid style="background: transparent;">
           <ion-row style="background: transparent;" color="primary" justify-content-center>
              <ion-col style="background: transparent;" align-self-center size-md="6" size-lg="5" size-xs="12">
                 <ion-card style="background: transparent; box-shadow: 0 13px 26px 58px rgba(0,0,0,.2), 0 16px 53px 0 rgba(0,0,0,.14), 0 1px 5px 0 rgba(0,0,0,.12); -webkit-box-shadow:  0 13px 26px 58px rgba(0,0,0,.2), 0 16px 53px 0 rgba(0,0,0,.14), 0 1px 5px 0 rgba(0,0,0,.12);">
                     <ion-card-header style="background: transparent;">
                        <ion-card-title style="color: white; font-weight: bolder;">{{ country && city ? city+', '+country : 'No country' }}</ion-card-title>
                     </ion-card-header>
                        <ion-item style="--background: transparent;" lines="none" text-center>
                            <ion-label style="--background: transparent; color: white; font-weight: bolder;">{{ weatherDescription ? weatherDescription : '' }}</ion-label>
                        </ion-item>
                        <ion-item style="display: -webkit-inline-box !important; margin: 0 auto!important; --background: transparent;" lines="none">
                            <ion-img  :src="icon" v-if="icon" alt="Weather Icon"></ion-img>
                            <ion-label style="--background: transparent; color: white; font-weight: bolder;">{{ temp ? temp+'°C' : ''}}</ion-label>
                        </ion-item>
                        <ion-item style="--background: transparent;" line="none">
                            <ion-label style="--background: transparent; color: white; font-weight: bolder;">{{ tempMin ? tempMin+'°C' : '' }}</ion-label>
                            <ion-label style="--background: transparent; color: white; font-weight: bolder;">{{ tempMax ? tempMax+'°C' : '' }}</ion-label>
                            <ion-label style="--background: transparent; color: white; font-weight: bolder;">{{ humidity ? 'Hum: '+humidity+'%' : '' }}</ion-label>
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
                API_KEY:'d63783958914771e7b1abff1566d9001',
                isDisabled: true,
                timeout: 3000
            }
        },
        methods:{
            getWeather(event){
                this.city=event.target[0].value
                if(this.city.trim() !== '')
                {
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
            },
           getCity(event)
           {
               console.log(event)
               event.detail.value.trim() !== '' ? this.isDisabled = false : this.isDisabled = true
           },
            presentLoading()
            {
                return this.$ionic.loadingController.create({
                        message: 'Loading',
                        duration: this.timeout,
                        spinner: 'bubbles',
                        cssClass: 'loading-class'
                    })
                    .then(l => {setTimeout(function() {
                                    l.dismiss()
                                }, this.timeout)
                        return l.present()
                    })
            },

    }
    }
</script>

<style scoped>
    .weather{
        background: url('../assets/weather_ground.jpg') no-repeat center;
        margin-top: -4%;
    }

    .item-highlight, .item-inner-highlight {
        left: 0;
        right: 0;
        bottom: 0;
        position: absolute;
        --background: green
    }

    .loading-class{
        --spinner-color: green;
    }
</style>