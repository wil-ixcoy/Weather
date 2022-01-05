<template>
  <div>
    <section class="">
      <div>
        <input type="text" placeholder="Buscar Ciudad" v-model="busqueda" />
        <img src="" alt="buscar" />
      </div>
      <button v-on:click.prevent="buscar">Buscar</button>
    </section>
    <h1>{{ infoClima.name }} / {{ infoClima.country }}</h1>
    <section class="W-container">
      <article class="W-subContainer">
        <section class="W-item">
          <img :src="infoClima.icon" alt="icon" class="W-image" />
          <h2>Clima: {{ infoClima.description }}</h2>
        </section>
      </article>

      <article class="W-subContainer">
        <section class="W-item">
          <img
            src="../assets/temp_actual.svg"
            alt="temperatura-actual"
            class="W-image"
          />
          <h2>Temperatura: {{ infoClima.temp }}Cº</h2>
        </section>

        <section class="W-item">
          <img
            src="../assets/temp_max.svg"
            alt="temperatura-max"
            class="W-image"
          />
          <h2>Temperatura maxima: {{ infoClima.temp_max }}Cº</h2>
        </section>

        <section class="W-item">
          <img
            src="../assets/temp_min.svg"
            alt="temperatura-minima"
            class="W-image"
          />
          <h2>Temperatura minima: {{ infoClima.temp_min }}Cº</h2>
        </section>
      </article>

      <article class="W-subContainer">
        <section class="W-item">
          <img src="../assets/wind.svg" alt="viento" class="W-image" />
          <h2>Viento: {{ infoClima.viento }} km/h</h2>
        </section>

        <section class="W-item">
          <img src="../assets/wind.svg" alt="rafaga" class="W-image" />
          <h2>Rafaga: {{ infoClima.rafaga }} km/h</h2>
        </section>
      </article>
    </section>
    <pronostico v-bind:msg="nombreCiudad"></pronostico>
  </div>
</template>
<script>
import { ref } from "vue";
import axios from "axios";
import pronostico from "./pronostico.vue";
export default {
  setup() {
    //variable que almacena el nombre  de ciudad
    let nombreCiudad = ref("");
    let busqueda = ref("");
    //arreglo con datos de la api
    const infoClima = ref({});

    function buscar() {
      nombreCiudad.value = busqueda.value;
      return URL(nombreCiudad.value);
    }
    URL(nombreCiudad.value);
    //funcion para obtener la informacion de la api
    function URL(data) {
      let ciudad = data === "" ? "Santa Cruz del Quiché" : data;
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${ciudad}&appid=235cfdfa708afcdf68cf0dffcfb036d6&lang=es&units=metric`
        )
        .then((response) => {
          console.log(response.data);
          //se agregan los datos al arreglo infoClima
          infoClima.value = {
            country: response.data.sys.country,
            name: response.data.name,
            description: response.data.weather[0].description,
            temp: response.data.main.temp,
            temp_max: response.data.main.temp_max,
            temp_min: response.data.main.temp_min,
            viento: response.data.wind.speed,
            rafaga: response.data.wind.gust,
            icon: `http://openweathermap.org/img/wn/${response.data.weather[0].icon}@2x.png`,
          };
          console.log(infoClima.value);
        })
        .catch((error) => {
          console.log(error);
        });
    }

    return {
      nombreCiudad,
      URL,
      busqueda,
      infoClima,
      buscar,
    };
  },

  components: {
    pronostico,
  },
};
</script>
