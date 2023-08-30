<script>
    export let currentWeather;
    export let dateToWords;
</script>

<div class="col-md-6">
    <div class="card rounded p-3 mb-3 cardeffects" style="width: 100%;">
        <div class="card-body cardeffects">
            <div class="row">
                <div class="col-md-12 text-center mb-3">
                    <h3 style="white-space: nowrap; font-weight: bold; font-size: 1.7rem;">{currentWeather.location.name}</h3>
                </div>

                <div class="col-md-5 text-center d-flex align-items-center justify-content-center">
                    <div>
                        <img 
                            src={`http:${currentWeather.condition.icon}`} 
                            alt={currentWeather.condition.text} 
                            style="max-width: 80%; height: auto;" 
                        />
                        <h2>
                            <span class="temp-value">{currentWeather.temp_c.toFixed(2)}°C</span>
                        </h2>
                    </div>
                </div>

                <div class="col-md-7">
                    <div style="max-width: 100%;">
                        <p style="font-size: 0.8rem;">{`${currentWeather.location.region}/${currentWeather.location.country}`}</p>
                        <p style="white-space: nowrap; font-size: 0.8rem;">{dateToWords(currentWeather.location.localtime)}</p>
                        <p style="font-size: 0.8rem;">{currentWeather.condition.text}</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
