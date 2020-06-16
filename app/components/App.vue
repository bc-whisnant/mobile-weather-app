<template>
    <Page>
        <ActionBar title="NativeScript-Vue Weather App"/>
        <StackLayout class="container" v-if="!submitted">
            <TextField hint="Enter city" v-model="form.query"/>
            <Button text="Search" @tap="searchWeather"/>
        </StackLayout>
        <StackLayout class="container" v-else>
            <TextView>
              In {{ name }} the weather is described as {{ description }}. The current temperature is {{ currentTemp }} degrees.
            The max today was {{ maxTemp }} and the min was {{ minTemp }}. It feels like it is {{feelsLike }} degrees.
            </TextView>
        </StackLayout>
    </Page>
</template>

<script >
  import axios from 'axios'
  export default {
    data() {
      return {
        form: {
          query: ''
        },
        API_KEY: "731509ea59cc8fb073325761658c3185",
        currentTemp: "",
        humidity: "",
        feelsLike: "",
        maxTemp: "",
        minTemp: "",
        name: "",
        description: "",
        submitted: false,
        valid: false
      }
    },
    methods: {
      searchWeather() {
        axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.form.query}&units=imperial&apikey=${this.API_KEY}`
        )
        .then((response) => {
          const {
            temp,
            humidity,
            feels_like,
            temp_max,
            temp_min,
          } = response.data.main;
          const { description } = response.data.weather[0];
          this.submitted = true;
          this.name = response.data.name;
          this.currentTemp = temp;
          this.humidity = humidity;
          this.feelsLike = feels_like;
          this.maxTemp = temp_max;
          this.minTemp = temp_min;
          this.description = description;
        });
      }
    }
  }
</script>

<style scoped>
    ActionBar,
    Button {
        background-color: #ff8C00;
        color: #ffffff;
    }

    Button {
      width: 65%;
    }

    .container {
        vertical-align: center;
        text-align: center;
        font-size: 80;
    }
</style>
