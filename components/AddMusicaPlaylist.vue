<template>
  <div>
    <div id="addMusicaPlaylist-form" class="menu menu-box-bottom menu-box-detached rounded-m">
      <div class="content">
        <div class="d-flex mb-2">
          <div>
            <h3 class="opacity-20">Adicionar Música na Playlist</h3>
          </div>
        </div>
        <div class="input-style has-borders no-icon validate-field mb-4 input-style-active">
          <input type="text" class="form-control validate-name" v-model="titulo" id="titulo" placeholder="Título da Música">
          <label for="titulo" class="color-blue-dark font-600 text-uppercase">Buscar Música</label>
          <i class="fa fa-times disabled invalid color-red-dark"></i>
          <i class="fa fa-check disabled valid color-green-dark"></i>
          <em>(Obrigatório)</em>
        </div>
        <div class="list-group list-custom-small icon-0">
          <a
            v-for="(item, key) in lista" :key="key"
            @click="addMusica(item.id)"
          >
            <i class="fa fa-sign-in-alt color-red-dark"></i>
            <span>{{ item.titulo }} - <em>{{ item.artista }}</em></span>
            <i class="fa fa-angle-right"></i>
          </a>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      titulo: null,
      lista: []
    }
  },

  watch: {
    titulo: function(value) {
      if (value.length <= 3) {
        return [];
      }
      this.$axios.$get('/buscar/' + value)
      .then((response) => {
        this.lista = response;
      });
    }
  },
  methods: {
    async addMusica(id) {
      this.$axios.$post('/playlist/add-musica', {
        playlist_id: this.$route.params.id,
        musica_id: id
      })
      .then((response) => {
        this.close();
        this.$emit('recarregar');
      });
    },
    close: function() {
      const activeMenu = document.querySelectorAll('.menu-active');
      for (let i=0; i < activeMenu.length; i++) {
        activeMenu[i].classList.remove('menu-active');
      }
    }
  }
}
</script>
