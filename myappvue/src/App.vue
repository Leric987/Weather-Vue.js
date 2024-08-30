<script >
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
			return "Температура:" + this.info.main.temp
		},
		showFeelsLike() {
			return "Ощущается как:" + this.info.main.feels_like
		},
		showMinTemp() {
			return "Минимальная температура:" + this.info.main.temp_min
		},
		showMaxTemp() {
			return "Максимальная температура:" + this.info.main.temp_max
		}
	},
	methods: {
		getWeather() {
			if(this.city.trim().length < 2) {
				this.error = "Нужно название более одноого символа"
				return false
			}

			this.error = ""

			axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=50d7e180d3d22a14b38ab7b4807ab35e`)
				.then(res => (this.info = res.data))
		}
	}
}
</script>

<template>
	<div class="wrapper">
		<h1>Погодное приложение</h1>
		<p>Узнать погоду в {{ city == "" ? "вашем городе" : cityName }}</p>
		<input type="text" v-model="city" placeholder="Введите город">
		<button v-if="city != ''" @click="getWeather()" >Получить погоду</button>
		<button disabled v-else >Введите название города</button>
		<p class="error">{{ error }}</p>

		<div v-if="info != null">
			<p>{{ showTemp }}</p>
			<p>{{ showFeelsLike }}</p>
			<p>{{ showMinTemp }}</p>
			<p>{{ showMaxTemp }}</p>
		</div>
		
	</div>
</template>

<style scoped>

.error {
	color: red;
}

.wrapper {
		width: 900px;
		height: 500px;
		border-radius: 50px;
		padding: 20px;
		background: #FF7F50;
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
	border-bottom: 2px solid #fcfcfc;
	color: #fcfcfc;
	font-size: 14px;
	padding: 5px 8px;
	outline: none;
}

.wrapper input:focus {
	border-bottom-color: darkcyan;
}

.wrapper button {
	background:cadetblue;
	color: #fcfcfc;
	border: 2px solid cyan;
	border-radius: 10px;
	padding: 10px 15px;
	margin-left: 20px;
	cursor: pointer;
	transition: transform 500ms ease;
}

.wrapper button:disabled  {
	background: gray;
	cursor: not-allowed;
}
.wrapper button:hover {
	transform: scale(1.1) translateY(-8px);
}


</style>
