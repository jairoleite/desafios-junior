<template>
  <q-page class="flex flex-center">
    <q-btn
      class="q-mr-md"
      round
      flat
      color="gray"
      icon="chevron_left"
      @click="prev"
    />

    <div class="container">
      <div class="calendario">
        <div
          class="dia"
          v-for="(semana, index) in semanaAtual.dias"
          :key="index"
          @click="clickDia(semana)"
        >
          <span
            class="numeral"
            :style="semana.today ? 'color: #ca5555;' : ''"
            >{{ semana.numeral }}</span
          >
          <span class="semana" :style="semana.today ? 'color: #ca5555;' : ''">{{
            semana.dia
          }}</span>
        </div>
      </div>
      <span>{{ semanaAtual.semana }}</span>
    </div>

    <q-btn
      class="q-ml-md"
      round
      flat
      color="gray"
      icon="navigate_next"
      @click="next"
    />
  </q-page>
</template>

<script>
import { defineComponent, ref } from "vue";

export default defineComponent({
  name: "PageIndex",
  setup() {
    //semana atual
    const semanaAtual = ref();
    //indíce atual
    const indiceAtual = ref();

    const calendario = ref([
      {
        semana: "27 a 31 de dezembro de 2021",
        atual: true,
        dias: [
          { dia: "Seg", today: false, numeral: "27" },
          { dia: "Ter", today: false, numeral: "28" },
          { dia: "Qua", today: true, numeral: "29" },
          { dia: "Qui", today: false, numeral: "30" },
          { dia: "Sex", today: false, numeral: "31" },
        ],
      },
      {
        semana: "03 a 07 de janeiro de 2022",
        atual: false,
        dias: [
          { dia: "Seg", today: false, numeral: "03" },
          { dia: "Ter", today: false, numeral: "04" },
          { dia: "Qua", today: false, numeral: "05" },
          { dia: "Qui", today: false, numeral: "06" },
          { dia: "Sex", today: false, numeral: "07" },
        ],
      },
      {
        semana: "10 a 14 de janeiro de 2022",
        atual: false,
        dias: [
          { dia: "Seg", today: false, numeral: "10" },
          { dia: "Ter", today: false, numeral: "11" },
          { dia: "Qua", today: false, numeral: "12" },
          { dia: "Qui", today: false, numeral: "13" },
          { dia: "Sex", today: false, numeral: "14" },
        ],
      },
    ]);
    //adiciona filtro de valor da semana atual
    semanaAtual.value = calendario.value.find((c) => c.atual == true);
    indiceAtual.value = calendario.value.findIndex((c) => c.atual == true);

    const prev = () => {
      if (indiceAtual.value > 0) {
        indiceAtual.value = indiceAtual.value - 1;

        console.log(indiceAtual.value);
        //pega item da lista
        calendario.value = calendario.value.map((c, index) => {
          if (index === indiceAtual.value) {
            c.atual = true;

            semanaAtual.value = c;
          } else {
            c.atual = false;
          }

          return c;
        });
      }
    };

    const next = () => {
      if (indiceAtual.value !== calendario.value.length - 1) {
        indiceAtual.value = indiceAtual.value + 1;

        calendario.value = calendario.value.map((c, index) => {
          if (index === indiceAtual.value) {
            c.atual = true;

            semanaAtual.value = c;
          } else {
            c.atual = false;
          }

          return c;
        });
      }
    };

    const clickDia = (semana) => {
      semanaAtual.value.dias.map((s) => {
        if (s === semana) {
          s.today = true;
        } else {
          s.today = false;
        }

        return s;
      });
    };

    return {
      semanaAtual,
      next,
      prev,
      clickDia,
    };
  },
});
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.container > span {
  margin-top: 20px;
}

.calendario {
  display: flex;
  margin-top: 35px;
}

.calendario > .dia {
  display: flex;
  flex-direction: column;
  align-items: center;

  margin: 0px 10px;
  cursor: pointer;
}

.dia:active {
  background-color: rgb(219, 219, 219);
  border-radius: 5px;
}

.dia > .numeral {
  font-size: 30px;
  font-weight: bold;
}

.dia > .semana {
  font-size: 16px;
  margin-top: -9px;
}
</style>
