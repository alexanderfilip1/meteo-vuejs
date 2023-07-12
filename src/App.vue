<script>
import axios from 'axios'
export default {
  data() {
    return {
      city: "",
      error: "",
			info: null
    }
  },
  computed: {
		cityName() {
				return "«" + this.city + "»"
		},
		showTemp() {
			return "Temperatura este: " + this.info.main.temp
		},
		showFeelsLike() {
		return "Feels like: " + this.info.main.feels_like
		},
		showMinTemp() {
			return "Temperatura minima: " + this.info.main.temp_min
		},
		showMaxTemp() {
			return "Temperatura maxima: " + this.info.main.temp_max
		},
					}	,
  methods: {
    getWeather() {
      if(this.city.trim().length < 2) {
        this.error = "Este nevoie de o denumire mai mare de 1 simbol"
        return false
      }
      this.error = ""
			
			axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=d667a4b97600761fabbaa819b02d97cf`)
						.then(res => (this.info = res.data))
    }
  }
}
</script>

<template>
  <div class="wrapper">
    <h1>Aplicatie Meteo</h1>
    <p>Afla vremea meteo din {{ city == "" ? "in orasul dvs" : cityName}}</p>
    <input type="text" v-model="city" placeholder="Scrie orasul">
    <button v-if="city != ''" @click="getWeather()">Vezi vremea</button>
    <button disable v-else>Scrie denumirea orasului</button>
    <p class="error">{{ error }}</p>
    
    <div v-if="info != null" class="showtemp">
    	 <p>{{ showTemp }}</p>
    	 <p>{{ showFeelsLike }}</p>
    	 <p>{{ showMinTemp }}</p>
    	 <p>{{ showMaxTemp }}</p>
    </div>
   
  </div>
</template>

<style scoped>
.error {
  color: #d03939;
}
.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background: sandybrown;
  text-align: center;
  color: #fff;
}

.wrapper h1 {
  margin-top: 50px;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid red;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: #4158D0;
}

.wrapper button:disabled {
  background: #FFCC70;
  cursor: crosshair;
}

.wrapper button {
  background: #4158D0;
  color: #fff;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}
	
.showtemp p {
	font-size: 20px
}
</style>
