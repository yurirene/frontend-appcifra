<template>
  <div>
    <div class="page-content">
      <div class="card card-style mb-3">
        <div class="content d-flex items-align-center">
          <div class="col">
            <h1>{{ musica?.titulo }}</h1>
            <p class="color-highlight font-12 mt-n3 pt-1 mb-2">
              {{ musica?.artista }}
            </p>
            <Metronomo :batimento="musica.bpm" />
          </div>
          <div class="col items-align-center">
            <button @click="back"
              class="btn btn-sm float-end rounded-xl shadow-xl text-uppercase font-800 bg-highlight"
            >
              <em class="fas fa-arrow-left ms-1"></em> Voltar
            </button>
            <button @click="baixar"
              class="btn btn-sm float-end rounded-xl shadow-xl text-white text-uppercase font-800 bg-primary"
              >
                <em class="fas fa-download ms-1"></em> Baixar
            </button>
          </div>
        </div>
      </div>
      <div class="card card-style mb-3 p-0">
        <div class="content">
          <span
            v-for="(ord, key) in ordem" :key="`ordem_${key}`"
            :style="`
              margin-left: 6px;
              border: 2px solid black;
              border-color: ${partes[ord]?.cor};
              font-weight: 800;
              border-radius: 50%;
              width: 25px;
              height: 25px;
              display:inline-flex;
              justify-content:center;
              align-items:center
          `">
            <span style="font-size: 12px;">{{ ord }}</span>
          </span>
          <br/>
          <p
            :style="`
              padding-left:3px;
              border-radius: 10px;
              border-left: 2px solid ${partes[ord]?.cor};
            `"
            v-for="(ord, key) in ordem" :key="key"
            class="mt-3"
          >
            <span v-html="partes[ord]?.cifra" ></span>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import Metronomo from '../../components/Metronomo.vue';


export default {
    data() {
        return {
            musica: [],
            ordem: [],
            partes: [],
        };
    },
    mounted() {
        this.$loadScript("/assets/scripts/custom.js")
            .then(() => { init_template(); });
        this.carregarCifra();
    },
    methods: {
      carregarCifra: function () {
          this.$axios.$get("/cifra/" + this.$route.params.id)
              .then((response) => {
                console.log(response.musica);
              this.ordem = response.ordem;
              this.partes = response.partes;
              this.musica = response.musica;
          });
      },
      back: function () {
          return this.$router.go(-1);
      },
      baixar: function () {
        this.$axios.$get("/baixar/" + this.$route.params.id)
            .then((response) => {
            const url = window.URL.createObjectURL(new Blob([response]));
            const link = document.createElement("a");
            link.href = url;
            link.setAttribute("download", this.musica.titulo + ".txt");
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            URL.revokeObjectURL(url);
        });
      },
    },
    components: { Metronomo }
}
</script>
