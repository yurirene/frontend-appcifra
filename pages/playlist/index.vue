<template>
  <div>
    <div class="page-content">
      <div class="card card-style">
        <div class="content mb-2">
          <h3>Lista de Playlist</h3>
          <p>
              Playlits Criadas.
          </p>
          <div class="list-group list-custom-small icon-0">
            <a
              v-for="(item, key) in lista" :key="key"
              :href="`/playlist/${item.id}`"
            >
              <i class="fa fa-sign-in-alt color-red-dark"></i>
              <span>{{ item.titulo }} - <small>{{ item.data_playlist }}</small></span>
              <i class="fa fa-angle-right"></i>
            </a>
          </div>
        </div>
      </div>

      <!-- <div class="card card-style">
        <div class="content">
          <div class="row mb-2 mt-n2">
            <div class="col-6 text-start">
              <h4 class="font-700 text-uppercase font-12 opacity-50 mt-1">Criar Playlist</h4>
            </div>
          </div>
          <div class="divider mt-4 mb-3"></div>
          <div class="d-flex">
            <div class="align-self-center w-100">
              <h5>Preencha o formulário e solicite ajuda</h5>
            </div>
          </div>
        </div>
        <a href="#"
          data-menu="solicitacao-form"
          class="btn btn-full btn-sm rounded-sm bg-highlight font-700 text-uppercase mt-3"
        >
          Fazer uma Solicitação
        </a>
      </div> -->
    </div>
  </div>
</template>
<script>

export default {
  data() {
    return {
      lista: []
    }
  },
  mounted() {
    this.$loadScript("/assets/scripts/custom.js")
    .then(()=> {init_template();})

    this.carregarLista();
  },
  methods: {
    carregarLista: function() {
      this.$axios.$get('/playlist')
      .then((response) => {
        this.lista = response;
      });
    }
  },
  computed: {
    parceiros: function() {
      return [];
    }
  }
}
</script>
