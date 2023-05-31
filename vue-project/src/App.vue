<template>
  <div>
    <h1>Data Visualization</h1>
    <div>
      <label for="city">City:</label>
      <input type="text" id="city" v-model="city" placeholder="Enter a city">
      <button @click="fetchData">Fetch Data</button>
    </div>
    <div>
      <canvas id="chart"></canvas>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      city: '',
      chartData: null,
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false,
      },
    };
  },
  methods: {
    async fetchData() {
      try {
        const response = await fetch(`=https://data.cityofnewyork.us/resource/dg92-zbpx.json/data?city=${this.city}`);
        const data = await response.json();

        // Process the data as necessary
        const labels = data.labels;
        const values = data.values;

        this.chartData = {
          labels: labels,
          datasets: [
            {
              label: 'Data',
              data: values,
              backgroundColor: 'rgba(75, 192, 192, 0.6)',
            },
          ],
        };

        this.createChart();
      } catch (error) {
        console.error(error);
      }
    },
    createChart() {
      const ctx = document.getElementById('chart').getContext('2d');

      new Chart(ctx, {
        type: 'bar',
        data: this.chartData,
        options: this.chartOptions,
      });
    },
  },
};
</script>

<style>
  /* Add your custom styles here */
</style>
