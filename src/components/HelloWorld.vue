<template>
    <div class='home'>
        <h1>主要都市の天気</h1>
        <b-container>
            <b-row>
                <b-col sm='12' class='mb-3'>
                    <hr>
                </b-col>
                <b-col sm='12' class='mx-auto mb-3' v-for="(city, key) in cities" :key="key">
                    <label sm="4" :id="key">{{ city.name }}</label>
                    <p sm="4" :id="key">{{ city.temp }}℃</p>
                    <img sm="4" v-if="city.weather" :src="`//openweathermap.org/img/wn/${city.weather}@2x.png`">
                    <b-icon v-else icon="dash"></b-icon>
                    <hr>
                </b-col>
            </b-row>
        </b-container>
    </div>
</template>
<script>
import axios from 'axios'

export default {
  name: 'home',
  components: {
  },
  data () {
    return {
      path: 'https://api.openweathermap.org/data/2.5/weather?appid=' + process.env.VUE_APP_WEATHER_APP_ID + '&id=',
      cities: { tokyo: { name: '東京', code: '1850152', weather: '', temp: 0 }, sapporo: { name: '札幌', code: '2128295', weather: '', temp: 0 }, sendai: { name: '仙台', code: '2111149', weather: '', temp: 0 }, osaka: { name: '大阪', code: '1853908', weather: '', temp: 0 }, nagoya: { name: '名古屋', code: '1856057', weather: '', temp: 0 }, hiroshima: { name: '広島', code: '1862415', weather: '', temp: 0 }, fukuoka: { name: '福岡', code: '1863967', weather: '', temp: 0 } }
    }
  },
  methods: {
    convertCelsius: (kelvin) => {
      return Math.floor(kelvin - 273.15)
    }
  },
  mounted () {
    if (!this.cities) return {}
    // リクエスト情報
    const request = {
      headers: {},
      response: true
    }
    // データ取得
    Object.keys(this.cities).forEach((key) => {
      let path = this.path + this.cities[key]['code']
      axios.get(path, request).then(response => {
        const res = response.data
        this.cities[key]['temp'] = this.convertCelsius(res.main.temp)
        this.cities[key]['weather'] = res.weather[0].icon
      }).catch(error => {
        console.log(error)
      })
    })
  }
}
</script>
