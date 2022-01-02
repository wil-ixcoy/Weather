<template>
  <div>
    <section>
      <div>
        <input type="text" placeholder="Buscar Ciudad" v-model="busqueda" />
        <img src="" alt="buscar" />
      </div>
      <button v-on:click.prevent="buscar">Buscar</button>
    </section>
    <h1>{{ infoClima.name }} / {{ infoClima.country }}</h1>
    <section>
      <h1>{{ busqueda }}/ {{ nombreCiudad }}</h1>
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

      <section>
        <img src="" alt="icon" />
        <h2>Viento: {{ infoClima.viento }} km/h</h2>
      </section>

      <section>
        <img src="" alt="icon" />
        <h2>Rafaga: {{ infoClima.rafaga }} km/h</h2>
      </section>
      <pronostico msg="Santa Cruz del Quiché"></pronostico>
    </section>
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
            icon: response.data.weather[0].icon,
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
