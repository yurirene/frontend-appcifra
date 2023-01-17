<template>
  <div>
    <div class="page-content">
      <div class="card card-style">
        <div class="content mb-2">
          <div class="row">
            <div class="col">
              <h3>Lista de Músicas</h3>
              <p>
                Músicas Cadastradas.
              </p>
            </div>

            <div class="col">
              <button
                data-menu="musica-form"
                class="btn btn-sm float-end rounded-xl shadow-xl text-uppercase font-800 bg-highlight">
                <em class="fas fa-plus ms-1"></em> Música
              </button>
            </div>
          </div>
          <div class="row">
            <div class="col">
              <input type="text" class="form-control" v-model="busca" />
            </div>
          </div>
          <div class="list-group list-custom-small icon-0">
            <nuxt-link
              v-for="(item, key) in lista" :key="key"
              :to="`/cadastro/${item.id}`"
            >
              <a href="#" >
                <i class="fa fa-sign-in-alt color-red-dark"></i>
                <span>{{ item.titulo }} - <em>{{ item.artista }}</em></span>
                <i class="fa fa-angle-right"></i>
              </a>
            </nuxt-link>
          </div>
        </div>
      </div>
    </div>

    <MusicaForm />
  </div>
</template>
<script>
import MusicaForm from '../../components/MusicaForm.vue';


export default {
    data() {
        return {
            lista: [],
            busca: null
        };
    },
    mounted() {
        this.$loadScript("/assets/scripts/custom.js")
            .then(() => { init_template(); });
        this.carregarLista();
    },
    methods: {
        carregarLista: function () {
            this.$axios.$get("/musicas")
                .then((response) => {
                this.lista = response;
            });
        }
    },
    watch: {
      busca: function(value) {
        if (value.length <= 3) {
          this.carregarLista();
          return;
        }
        this.$axios.$get('/buscar/' + value)
        .then((response) => {
          this.lista = response;
        });
      }
    },
    computed: {
        parceiros: function () {
            return [];
        }
    },
    components: { MusicaForm }
}
</script>
