<script>
  import { onMount, afterUpdate } from 'svelte';
  import Chart from 'chart.js/auto';
  
  export let nineTemps = [];
  
  const labels = ["00:00", "03:00", "06:00", "09:00", "12:00", "15:00", "18:00", "21:00", "24:00"];
  let chart;
  
  const commonFont = {
    family: '"Fira Sans", sans-serif',
    size: 15
  };
  
  const commonOptions = {
    responsive: true,
    scales: {
      y: { display: false },
      x: {
        ticks: { color: "#fff", font: {...commonFont, weight: 600} },
        grid: { display: true, color: "rgba(255, 255, 255, 0.1)" },
      },
    },
    plugins: {
      tooltip: {
        enabled: true,
        titleFont: commonFont,
        bodyFont: commonFont,
        backgroundColor: "rgba(0, 0, 0, 0.8)",
        padding: 20,
        caretSize: 10,
        displayColors: false,
      },
      legend: { display: false }
    }
  };
  
  onMount(() => {
    const ctx = document.getElementById('temp-chart-canvas').getContext('2d');
    const data = {
      labels,
      datasets: [{
        tension: 0.35,
        label: "Temperature (°C)",
        data: nineTemps,
        borderColor: "#FF0000",
        backgroundColor: "rgba(73, 133, 224, 0.5)",
        borderWidth: 5,
        radius: 3,
        hoverRadius: 10,
        hitRadius: 100,
        pointStyle: "circle",
      }]
    };
    
    chart = new Chart(ctx, {
      type: "line",
      data,
      options: commonOptions
    });
  });
  
  afterUpdate(() => {
    chart.data.datasets[0].data = nineTemps;
    chart.update();
  });
</script>
  
  <style>
  </style>
  
  <div class="temp-chart">
    <div class="temp-chart-heading"><h2>Hourly Forecast</h2></div>
    <canvas id="temp-chart-canvas" height="50" width="200"></canvas>
  </div>
  