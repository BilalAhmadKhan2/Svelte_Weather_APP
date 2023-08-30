<script>
    export let forecastData = [];
  
    const getDayName = (dateString) => {
      const date = new Date(dateString);
      const days = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
      return days[date.getDay()];
    };
  </script>
  
  <style>

  </style>
  
  <div class="forecast-list">
    <ul>
      {#each forecastData as day, index}
        <li>
          <span>{getDayName(day.date)}</span>
          <img src={`http:${day.day.condition.icon}`} alt={day.day.condition.text} />
          <span>H {day.day.maxtemp_c.toFixed(2)}°C</span>
          <span>L {day.day.mintemp_c.toFixed(2)}°C</span>
        </li>
      {/each}
    </ul>
  </div>
  