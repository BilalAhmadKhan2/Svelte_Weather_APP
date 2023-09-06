<script>
    import axios from 'axios';
    import { onMount } from 'svelte';
    import { API_KEY, API_URL } from './constants.js'; 
    import Currentweather from './Currentweather.svelte';
    import WeatherData from './WeatherData.svelte';
    import Forecast from './Forecast.svelte';
    import TempChart from './TempChart.svelte';

	let query = "", currentWeather, forecastData = [], errorMessage, loading = false, eightTemps = [], nineTemps = [], isDaytime;

	const dateToWords = localtime => {
		const [date, time] = localtime.split(" ");
		const [year, month, dateNum] = date.split("-");
		return `${["January","February","March","April","May","June","July","August","September","October","November","December"][parseInt(month) - 1]} ${dateNum}, ${year} ${time}`;
	};

	const search = async () => {
		loading = true;
		try {
			const { data: { current, location, forecast } } = await axios.get(`${API_URL}?key=${API_KEY}&q=${query}&days=3&aqi=yes&alerts=no`);
			if (current && location) {
				currentWeather = { ...current, location };
				forecastData = forecast?.forecastday || [];
				eightTemps = forecastData.length ? forecastData[0].hour.map(hour => hour.temp_c).filter((_, i) => i % 3 === 0) : [];
				nineTemps = [...eightTemps, forecastData.length ? forecastData[0].hour.slice(-1)[0].temp_c : null];
				isDaytime = current.is_day === 1;
				errorMessage = null;
			} else throw new Error("Location or weather data not found");
		} catch (error) {
			errorMessage = error.message;
			currentWeather = forecastData = null;
		} finally {
			loading = false;
		}
	};

	onMount(() => {
		query = "leeds";
		search();
	});
</script>

<div class="App container-fluid">
    <header class="mb-3 py-3">
        <div class="container">
            <div class="row justify-content-center">
                <div class="col-lg-6 col-md-8">
                    <div class="input-group input-group-sm mb-3">
                        <input 
                            type="text" 
                            class="form-control" 
                            placeholder="Search city..." 
                            aria-label="City name" 
                            bind:value={query} 
                        />
                        <button 
                            class="btn btn-outline-primary" 
                            type="button" 
                            on:click={search} 
                            style="border: none;">
                            <img 
                                src="https://cdn-icons-png.flaticon.com/512/954/954591.png" 
                                alt="Search Icon" 
                                style="height: 20px; width: 20px;" 
                            />
                        </button>
                    </div>
                </div>
            </div>
            <div class="container">
                <div class="row justify-content-center">
                    <div class="col-lg-6 col-md-8">
                        {#if loading}
                            <p class='text-center mt-3'>Loading...</p>
                        {/if}
                        {#if errorMessage}
                            <p class="text-danger text-center mt-3">{errorMessage}</p>
                        {/if}
                    </div>
                </div>
            </div>
        </div>
    </header>
    {#if currentWeather}
        <div class="row mt-3">
            <div class="col-lg-6 col-md-12 p-1">
                <div class="card rounded p-1 mb-2 " style="background-color: transparent;">
                    <div class="card-body">
                        <div class="row mb-4">
                            <Currentweather currentWeather={currentWeather} dateToWords={dateToWords} />
                            <WeatherData weatherData={currentWeather} />
                        </div>

                        {#if forecastData.length > 0}
                            <div class="row">
                                <div class="col">
                                    <div class="card rounded p-3 cardeffects">
                                        <div class="card-body cardeffects">
                                            <div class="col-12 p-3">
												<h2>Forecast</h2>
                                               <Forecast forecastData={forecastData} />

                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        {/if}
                    </div>
                </div>
            </div>
            
            <div class="col-lg-6 col-md-12 p-1">
                <div class="row">
                    <div class="col-12 mb-2">
                      <TempChart {nineTemps} />
                    </div>
                    <div class="col-12">
                        <img 
						src={isDaytime ? "https://images.wallpaperscraft.com/image/single/clouds_sky_day_172438_1280x720.jpg" : "https://images.hdqwalls.com/download/mountains-night-sky-1280x720.jpg"} 
						alt={isDaytime ? "Daytime Landscape" : "Nighttime Landscape"} 
						style="width: 100%; height: auto; border-radius: 20px;"
                        />
                    </div>
                </div>
            </div>
        </div>
    {/if}
    <footer class="mt-3 pt-3 text-center">
        <p style="color: #FFFFFF;">
            © 2023 Weather App. All rights reserved. Designed and built by Bilal Ahmad Khan with Svelte, Bootstrap, CSS, Chart.js, and WeatherAPI.
        </p>
    </footer>
</div>
