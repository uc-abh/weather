<script>
	import { onMount } from "svelte";
	import axios from 'axios';


	let city_name="INDIA";
	let weather_data = {};
	let location_one = '';
	let location_two = '';
	onMount(() => {
		getWeatherData();
	});
	function getWeatherData() {
		// console.log(city_name);
		const options = {
			method: 'GET',
			url: 'https://weatherapi-com.p.rapidapi.com/current.json',
			params: {q: city_name},
			headers: {
				'X-RapidAPI-Key': '3cb029bf8dmsh413fc0896a94b5ep18416djsn3705df7e48b5',
				'X-RapidAPI-Host': 'weatherapi-com.p.rapidapi.com'
			}
		};

		axios.request(options).then(function (response) {
			weather_data = response.data;
			location_one = (weather_data?.location?.country).toLowerCase();
			location_two = (weather_data?.location?.name).toLowerCase();
			// console.log('response', response.data);
		}).catch(function (error) {
			// console.error(error);
		});
	}
</script>
<main>
<div id="container">
	<div class="card text-white bg-primary mb-3 border-4 border-dark" style="max-width: 40rem; min-width:30rem; min-height:75vh;">
		<div class="card-header d-flex justify-content-center">
			<input type="Search" bind:value={city_name} on:change={() => getWeatherData()} placeholder="Enter your city or country name and press enter"  minlength="1" maxlength="23" required/>
		</div>
		<!-- {location_one} {city_name.toLowerCase()} -->
	{#if location_one == city_name.toLowerCase() || location_two == city_name.toLowerCase()}
	<div class="card-body">
			
		<h1 class="card-title d-flex justify-content-center text-uppercase countryname"><i class="fa-solid fa-street-view"></i>&nbsp{city_name} &nbsp <i class="fa-solid fa-street-view"></i></h1>
		<h3 class="card-text d-flex justify-content-center">
		{weather_data?.current?.temp_c}℃
		</h3>
		<h4 class="d-flex justify-content-center">{weather_data?.current?.last_updated}</h4>
		<div class="d-flex justify-content-center">
		{#if weather_data?.current?.is_day ===1}	
			<img src="../images/sun.png" class="mt-4" alt="Day" srcset="" width="100px" >
		{:else}
		<img src="../images/moon.png" class="mt-4" alt="Night" srcset=""  width="100px" >

		{/if}
		</div>
	</div>

{:else}

	<div class="d-flex justify-content-center mt-2">
<h5 class="mt-4">Data not found please enter valid details !!!!</h5>
   </div>
   <div class="d-flex justify-content-center">
<img src="../images/sad_emoji.png" class="mt-4" width="200px" alt="" srcset="">

</div>

	{/if}

		
	</div>
</div>

</main>

<style>
    #container{
        width: 100vw;
        height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
    }

	input[type=Search]{
  border-radius:10px;
  width: 28vw;
  height: 35px;
  border: 3px solid black;
}
</style>