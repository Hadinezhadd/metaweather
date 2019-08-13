<template>
<div>
   <b-navbar toggleable="lg" type="dark" variant="primary">
    <b-navbar-brand href="#">MetaWeather</b-navbar-brand>
    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
        <b-nav-item href="#">Home</b-nav-item>
        <b-nav-item-dropdown text="Language" right>
          <b-dropdown-item href="#">EN</b-dropdown-item>
          <b-dropdown-item href="#">ES</b-dropdown-item>
          <b-dropdown-item href="#">RU</b-dropdown-item>
          <b-dropdown-item href="#">FA</b-dropdown-item>
        </b-nav-item-dropdown>
        <b-nav-item-dropdown text="City" right>
          <b-dropdown-item href="#" id="2487956" v-on:click="change">San Francisco</b-dropdown-item>
          <b-dropdown-item href="#" id="2459115" v-on:click="change">New York</b-dropdown-item>
          <b-dropdown-item href="#" id="44418" v-on:click="change">London</b-dropdown-item>
          <b-dropdown-item href="#" id="1118370" v-on:click="change">Tokyo</b-dropdown-item>
        </b-nav-item-dropdown>
        <b-nav-item href="#">API</b-nav-item>
         <b-nav-item href="#">About</b-nav-item>
      </b-navbar-nav>
      <b-navbar-nav class="ml-auto">
        <b-nav-form>
          <b-form-input size="sm" class="mr-sm-2" placeholder="Search"></b-form-input>
          <b-button size="sm" class="my-2 my-sm-0" type="submit">Search</b-button>
        </b-nav-form>
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
<b-container fluid class="bv-example-row">

  <div class="title">
     <span > {{ info.title }}</span><span style="font-size:13px">.{{info.parent.title}}</span>
  </div>
 
<div>
  <b-card-group deck class="text-center">
    <b-card  
    :key="index" v-for="(item,index) in info.consolidated_weather"
     >
     <div v-if="index == '0'" class="day-title">
        <P>Today</P>
     </div>
     <div v-else-if="index == '1'" class="day-title">
        <P>Tomorrow</P>
     </div>
     <div v-else class="day-title">
        <P>{{info.consolidated_weather[index].applicable_date}}</P>
     </div>

     <img v-bind:src="'https://www.metaweather.com/static/img/weather/'+info.consolidated_weather[index].weather_state_abbr+'.svg'" /> 
      <b-card-text class="weather_state">
        {{info.consolidated_weather[index].weather_state_name}}
      </b-card-text>
       <b-card-text class="m_temp">
        Max: {{info.consolidated_weather[index].max_temp.toFixed(0)}} °C<br>
        Min: {{info.consolidated_weather[index].min_temp.toFixed(0)}} °C
      </b-card-text>
      <div slot="footer"><small class="text-muted"> {{info.consolidated_weather[index].wind_speed.toFixed(0)}}</small><small class="text-muted"> {{info.consolidated_weather[index].wind_direction_compass}}</small></div>
    </b-card>
  </b-card-group>
</div>
</b-container>
</div>
</template>

<script>
export default {
  name: 'Weather',
  props: {
    msg: String
  },
   data () {
    return {
      info: null,
      woeid:2251945,
    }
  },
    methods: {
    change: function (event) {
      this.woeid = event.target.id
      if (event) {
     fetch('http://localhost/weather/?command=location&woeid='+this.woeid)
    .then(response => 
        response.json().then(data => ({
        data: data,
        status: response.status
    })
).then(res => {
    this.info = res.data;
}));
      }
    }
  },
  mounted(){
     fetch('http://localhost/weather/?command=location&woeid='+this.woeid)
    .then(response => 
        response.json().then(data => ({
        data: data,
        status: response.status
    })
).then(res => {
    this.info = res.data;
}));
  }
}
</script>

<style scoped>
@import '../assets/css/Weather.scss';
</style>
