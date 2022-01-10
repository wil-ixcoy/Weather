<template>
  <div>
    <h1>Clima en los proximos dias</h1>
    <div v-if="filtroDatos.ciudad">
      <p class="W-texto_guia">{{ errorCiudad }}</p>
    </div>
    <div v-else>
      <p class="W-error_texto">{{ errorCiudad }}</p>
    </div>
    <section class="W-container_row">
      <section
        v-for="(dia, index) in filtroDatos"
        :key="dia[index]"
        class="W-border_item_card"
      >
        <h3 class="W-dia_correction">{{ dia.dia }}</h3>
        <div class="W-row_card">
          <img :src="dia.icon" alt="icono_clima" class="W-image_card" />
          <p>Clima: {{ dia.clima }}</p>
        </div>
        <div class="W-row_card">
          <img
            src="../assets/temp_actual.svg"
            alt="temperatura"
            class="W-image_card"
          />
          <p>Temperatura: {{ dia.temp }} Cº</p>
        </div>
        <div class="W-row_card">
          <img src="../assets/temp_max.svg" alt="maxima" class="W-image_card" />
          <p>Maxima: {{ dia.temp_max }} Cº</p>
        </div>
        <div class="W-row_card">
          <img src="../assets/temp_min.svg" alt="minima" class="W-image_card" />
          <p>Minima: {{ dia.temp_min }} Cº</p>
        </div>
        <div class="W-row_card">
          <img src="../assets/viento.svg" alt="viento" class="W-image_card" />
          <p>Viento: {{ dia.viento }} Km/h</p>
        </div>
        <div class="W-row_card">
          <img src="../assets/rafaga.svg" alt="rafaga" class="W-image_card" />
          <p>Rafaga: {{ dia.rafaga }} Km/h</p>
        </div>
      </section>
    </section>
  </div>
</template>

<script>
import axios from "axios";
import { ref, watch } from "vue";
export default {
  props: ["msg"],
  setup(props) {
    //almacena todos los dato de la api
    let datos = ref([]);
    //muestra error en pantalla
    let errorCiudad = ref("");
    //datos filtrados por el dia
    let filtroDatos = ref([]);
    //asignamos un origen por defecto
    let origen = ref("Santa Cruz del Quiché");
    pronosticoSemana(origen.value);
    watch(
      () => props.msg,
      (newValue) => {
        pronosticoSemana(newValue);
      }
    );

    function pronosticoSemana(data) {
      errorCiudad.value = "";
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
                ciudad: datos.value.city.name,
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

        .catch(() => {
          errorCiudad.value =
            "Verifique que el nombre de la ciudad este escrito de manera correcta";
        });
    }

    return {
      pronosticoSemana,
      datos,
      filtroDatos,
      origen,
      errorCiudad,
    };
  },
};
</script>
