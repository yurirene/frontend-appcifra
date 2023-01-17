<template>
  <div>
    <div class="page-content">
      <div class="card card-style">
        <div class="content">
          <h1>{{ playlist?.titulo }}</h1>
          <p class="color-highlight font-12 mt-n3 pt-1 mb-2">
            {{ playlist?.data_playlist }}
          </p>
        </div>
      </div>
      <div class="card card-style">
        <div class="content mb-0" id="tab-group-1">
          <h3>Músicas</h3>
          <div class="clearfix mb-3"></div>
          <div class="list-group list-custom-small icon-0">
            <a
              v-for="(item, key) in lista" :key="key"
              :href="'/cifra/' + item.id"
            >
              <i class="fa fa-music color-red-dark"></i>
              <span>{{ item.titulo }} - <em>{{ item.artista }}</em></span>
              <i class="fa fa-angle-right"></i>
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>

export default {
  data() {
    return {
      lista: [],
      playlist: null
    }
  },
  mounted() {
    this.$loadScript("/assets/scripts/custom.js")
    .then(()=> {init_template();})

    this.carregarLista();
  },
  methods: {
    carregarLista: function() {
      this.$axios.$get('/playlist/' + this.$route.params.id)
      .then((response) => {
        this.lista = response.musicas;
        this.playlist = response.playlist;
      });
    }
  }
}
</script>
