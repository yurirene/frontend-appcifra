<template>
  <div>
    <div class="page-content">
      <div class="card card-style">
        <div class="content mb-2">
          <h2>Cifras</h2>
          <p>
            Lista de Cifras.
          </p>
          <div class="list-group list-custom-small list-icon-0">
            <a href="#" v-for="(parceiro, key) in parceiros" :key="key" >
              <i class="fas font-14 fa-people-carry rounded-s color-facebook"></i>
              <span>{{ parceiro.nome }}</span>
              <span class="badge text-uppercase bg-green-dark">
                {{ `${parceiro.quantidade_vagas} vaga${parceiro.quantidade_vagas != 1 ? 's' : '' }` }}
              </span>
              <i class="fa fa-chevron-right disabled"></i>
            </a>
          </div>
        </div>
      </div>

      <div class="card card-style">
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
      </div>
    </div>
  </div>
</template>
<script>

export default {
  mounted() {
    this.$loadScript("/assets/scripts/custom.js")
    .then(()=> {init_template();})
  },
  computed: {
    parceiros: function() {
      return [];
    }
  }
}
</script>
