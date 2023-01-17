<template>
  <div>
    <div id="playlist-form" class="menu menu-box-bottom menu-box-detached rounded-m">
      <div class="content">
        <div class="d-flex">
          <div>
            <h1 class="opacity-20">Nova Playlist</h1>
          </div>
        </div>
        <div class="input-style has-borders no-icon validate-field mb-4 input-style-active">
          <input type="text" class="form-control validate-name" id="titulo" placeholder="titulo">
          <label for="titulo" class="color-blue-dark font-600 text-uppercase">Título</label>
          <i class="fa fa-times disabled invalid color-red-dark"></i>
          <i class="fa fa-check disabled valid color-green-dark"></i>
          <em>(Obrigatório)</em>
        </div>

        <div class="input-style has-borders no-icon validate-field mb-4 input-style-active">
          <input type="text" class="form-control validate-name" id="dia" placeholder="dia">
          <label for="dia" class="color-blue-dark font-600 text-uppercase">Data</label>
          <i class="fa fa-times disabled invalid color-red-dark"></i>
          <i class="fa fa-check disabled valid color-green-dark"></i>
          <em>(Obrigatório)</em>
        </div>

        <a href="#"
          class="close-menu btn btn-full btn-m rounded-m bg-green-dark font-700 text-uppercase mb-2"
          @click="cadastrar"
        >
          Finalizar
        </a>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      musica: {
        titulo: null,
        dia: null,
      },
    }
  },
  methods: {
    async cadastrar() {
      this.prepararSolicitacao();
      this.$axios.$post('/playlist/nova', this.musica)
      .then((response) => {
        console.log(response);
        this.$nuxt.$options.router.push('/playlist/'  + response.id);
      });
    },
    limpar() {
      this.musica = {
        titulo: null,
        dia: null,
      }
    },
    prepararSolicitacao: function() {
      this.musica.titulo = document.querySelector('#titulo').value
      this.musica.dia = document.querySelector('#dia').value
    }
  }
}
</script>
