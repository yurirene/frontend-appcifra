<template>
  <div>
    <div class="page-content">
      <div class="card card-style">
        <div class="content">

          <div class="row">
            <div class="col">
              <h1>{{ playlist?.titulo }}</h1>
              <p class="color-highlight font-12 mt-n3 pt-1 mb-2">
                {{ playlist?.data_playlist }}
              </p>
            </div>

            <div class="col">
              <button
                data-menu="addMusicaPlaylist-form"
                class="btn btn-sm float-end rounded-xl shadow-xl text-uppercase font-800 bg-highlight">
                <em class="fas fa-plus ms-1"></em> Música
              </button>
            </div>
          </div>
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
              @click.right="removerMusica(item.id)"
            >
              <i class="fa fa-music color-red-dark"></i>
              <span>{{ item.titulo }} - <em>{{ item.artista }}</em></span>
              <i class="fa fa-angle-right"></i>
            </a>
          </div>
        </div>
      </div>
    </div>
    <AddMusicaPlaylist @recarregar="carregarLista" />
  </div>
</template>
<script>
import AddMusicaPlaylist from '../../components/AddMusicaPlaylist.vue';


export default {
    data() {
        return {
            lista: [],
            playlist: null
        };
    },
    mounted() {
        this.$loadScript("/assets/scripts/custom.js")
            .then(() => { init_template(); });
        this.carregarLista();
    },
    methods: {
      carregarLista: function () {
          this.$axios.$get("/playlist/" + this.$route.params.id)
              .then((response) => {
              this.lista = response.musicas;
              this.playlist = response.playlist;
          });
      },
      async removerMusica(id) {
        this.$axios.$post('/playlist/remover-musica', {
          playlist_id: this.$route.params.id,
          musica_id: id
        })
        .then((response) => {
          this.close();
          this.carregarLista();
        });
      },
      close: function() {
        const activeMenu = document.querySelectorAll('.menu-active');
        for (let i=0; i < activeMenu.length; i++) {
          activeMenu[i].classList.remove('menu-active');
        }
      }
    },
    components: { AddMusicaPlaylist }
}
</script>
