<template>
  <div>
    <section>
      <div>
        <input v-model="nombreCiudad" type="text" placeholder="Buscar Ciudad" />
        <img src="" alt="buscar" />
      </div>
      <button v-on="URL(nombreCiudad)">Buscar</button>
    </section>
    <h1>{{ infoClima.name }} / {{ infoClima.country }}</h1>
    <section>
      <section>
        <img src="" alt="icon" />
        <h2>{{ infoClima.description }}</h2>
      </section>

      <section>
        <img src="" alt="icon" />
        <h2>Temperatura: {{ infoClima.temp }}Cº</h2>
      </section>

      <section>
        <img src="" alt="icon" />
        <h2>Temperatura maxima: {{ infoClima.temp_max }}Cº</h2>
      </section>

      <section>
        <img src="" alt="icon" />
        <h2>Temperatura minima: {{ infoClima.temp_min }}Cº</h2>
      </section>
    </section>
  </div>
</template>
<script>
import { ref } from "vue";
import axios from "axios";
export default {
  setup() {
    let nombreCiudad = ref("");

    const infoClima = ref({});

    URL(nombreCiudad.value);
    function URL(data) {
      let ciudad = data === "" ? "Guatemala" : data;
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${ciudad}&appid=235cfdfa708afcdf68cf0dffcfb036d6&lang=es&units=metric`
        )
        .then((response) => {
          console.log(response.data);
          infoClima.value = {
            name: response.data.name,
            country: response.data.sys.country,
            description: response.data.weather[0].description,
            temp: response.data.main.temp,
            temp_max: response.data.main.temp_max,
            temp_min: response.data.main.temp_min,
            icon: response.data.weather[0].icon,
          };
        })
        .catch((error) => {
          console.log(error);
        });
    }
    return {
      nombreCiudad,
      URL,
      infoClima,
    };
  },
};
/*

    const location = navigator.geolocation;
    function geolocation(pos) {
      let latiude = ref(pos.coords.latitude);
      let longitude = ref(pos.coords.longitude);
      //  console.log(latiude.value);
      //console.log(longitude.value);
      return {
        latiude,
        longitude,
      };
    }
    location.getCurrentPosition(geolocation);

    function getWeather() {
      axios(
        "https://api.openweathermap.org/data/2.5/weather?lat=15.0236312&lon=-91.1496969&appid=235cfdfa708afcdf68cf0dffcfb036d6"
      )
        .then((response) => {
          console.log(response.data);
        })
        .catch((error) => {
          console.log(error);
        });
    }
*/
</script>
