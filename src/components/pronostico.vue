<template>
  <div>
    <h1>Clima en los proximos dias en:</h1>
    <section v-for="(dia, index) in filtroDatos" :key="dia[index]">
      <h1>{{ dia.dia }}</h1>
      <img :src="dia.icon" alt="icno" />
      <p>Clima: {{ dia.clima }}</p>
      <p>Temperatura: {{ dia.temp }} Cº</p>
      <p>Maxima: {{ dia.temp_max }} Cº</p>
      <p>Minima: {{ dia.temp_min }} Cº</p>
      <p>Viento: {{ dia.viento }} Km/h</p>
      <p>Rafaga: {{ dia.rafaga }} Km/h</p>
    </section>
  </div>
</template>

<script>
import axios from "axios";
import { ref, watch } from "vue";
export default {
  props: ["msg"],
  setup(props) {
    let datos = ref([]);
    let filtroDatos = ref([]);
    console.log(props);
    let origen = ref("Santa Cruz del Quiché");
    pronosticoSemana(origen.value);
    watch(
      () => props.msg,
      (newValue, oldValue) => {
        console.log(oldValue, newValue);
        pronosticoSemana(newValue);
      }
    );

    function pronosticoSemana(data) {
      filtroDatos.value = [];
      let ciudad = data;

      axios
        .get(
          `https://api.openweathermap.org/data/2.5/forecast?q=${ciudad}&appid=235cfdfa708afcdf68cf0dffcfb036d6&lang=es&units=metric`
        )
        .then((response) => {
          datos.value = response.data;
          for (let i = 0; i < datos.value.list.length; i++) {
            if (datos.value.list[i].dt_txt.includes("18:00:00")) {
              filtroDatos.value.push({
                clima: datos.value.list[i].weather[0].description,
                dia: datos.value.list[i].dt_txt.substring(0, 10),
                hora: datos.value.list[i].dt_txt.substring(11, 16),
                temp: datos.value.list[i].main.temp,
                temp_max: datos.value.list[i].main.temp_max,
                temp_min: datos.value.list[i].main.temp_min,
                viento: datos.value.list[i].wind.speed,
                rafaga: datos.value.list[i].wind.gust,
                icon: `http://openweathermap.org/img/wn/${datos.value.list[i].weather[0].icon}@2x.png`,
              });
            }
          }
        })

        .catch((error) => {
          console.log(error);
        });
    }

    return {
      pronosticoSemana,
      datos,
      filtroDatos,
      origen,
    };
  },
};
</script>
