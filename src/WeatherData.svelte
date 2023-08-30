<script>
    export let weatherData;

    const icons = {
        pressure: 'https://icons.veryicon.com/png/o/miscellaneous/intelligent-agriculture/pressure-1.png',
        humidity: 'https://cdn-icons-png.flaticon.com/512/727/727790.png',
        windspeed: 'https://icon-library.com/images/wind-speed-icon/wind-speed-icon-6.jpg',
        uvindex: 'https://cdn-icons-png.flaticon.com/512/3262/3262975.png',
        winddir: 'https://cdn-icons-png.flaticon.com/512/2830/2830040.png',
        airquality: 'https://cdn-icons-png.flaticon.com/512/5024/5024476.png',
        feelslike: 'https://static-00.iconduck.com/assets.00/temperature-feels-like-icon-495x512-ylzv705f.png',
        visibility: 'https://cdn-icons-png.flaticon.com/512/3395/3395544.png'
    };

    const renderData = (icon, value) => ({ iconSrc: icons[icon], value });

    $: leftData = [
        renderData('pressure', `${weatherData.pressure_mb}mb`),
        renderData('humidity', `${weatherData.humidity}%`),
        renderData('windspeed', `${weatherData.wind_kph}kph`),
        renderData('uvindex', weatherData.uv)
    ];

    $: rightData = [
        renderData('winddir', weatherData.wind_dir),
        renderData('airquality', weatherData.air_quality['gb-defra-index']),
        renderData('feelslike', `${weatherData.feelslike_c.toFixed(2)}°C`),
        renderData('visibility', `${weatherData.vis_km}km`)
    ];
</script>

<div class="col-md-6">
    <div class="row">
        <div class="col-6 mb-3">
            <div class="card rounded p-3 cardeffects">
                <div class="card-body cardeffects">
                    {#each leftData as data}
                        <div class="d-flex align-items-center justify-content-start mb-2">
                            <div class="icon-container">
                                <img src={data.iconSrc} alt="" style="width: 38px; height: 38px;" />
                            </div>
                            <div class="value-container ml-2">
                                <p>{data.value}</p>
                            </div>
                        </div>
                    {/each}
                </div>
            </div>
        </div>
        
        <div class="col-6 mb-3">
            <div class="card rounded p-3 cardeffects">
                <div class="card-body cardeffects">
                    {#each rightData as data}
                        <div class="d-flex align-items-center justify-content-start mb-2">
                            <div class="icon-container">
                                <img src={data.iconSrc} alt="" style="width: 38px; height: 38px;" />
                            </div>
                            <div class="value-container ml-2">
                                <p>{data.value}</p>
                            </div>
                        </div>
                    {/each}
                </div>
            </div>
        </div>
    </div>
</div>

<style>

</style>



