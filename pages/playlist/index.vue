<template>
  <div>
    <div class="page-content">
      <div class="card card-style">
        <div class="content mb-2">
          <div class="row">
            <div class="col">
              <h3>Lista de Playlist</h3>
              <p>
                Playlits Criadas.
              </p>
            </div>

            <div class="col">
              <button
                data-menu="playlist-form"
                class="btn btn-sm float-end rounded-xl shadow-xl text-uppercase font-800 bg-highlight">
                <em class="fas fa-plus ms-1"></em> Playlist
              </button>
            </div>
          </div>

          <div class="list-group list-custom-small icon-0">
            <a
              v-for="(item, key) in lista" :key="key"
              :href="`/playlist/${item.id}`"
              @click.right="removerPlaylist(item.id)"
            >
              <i class="fa fa-sign-in-alt color-red-dark"></i>
              <span>{{ item.titulo }} - <small>{{ item.data_playlist }}</small></span>
              <i class="fa fa-angle-right"></i>
            </a>
          </div>
        </div>
      </div>
      <PlaylistForm />
    </div>
  </div>
</template>
<script>
import PlaylistForm from '../../components/PlaylistForm.vue';


export default {
    data() {
        return {
            lista: []
        };
    },
    mounted() {
        this.$loadScript("/assets/scripts/custom.js")
            .then(() => { init_template(); });
        this.carregarLista();
    },
    methods: {
      carregarLista: function () {
        this.$axios.$get("/playlist")
            .then((response) => {
            this.lista = response;
        });
      },
      removerPlaylist: function(id) {
        this.$axios.$post('/playlist/remover', {
          playlist_id: id,
        })
        .then((response) => {
          this.carregarLista();
        });
      }
    },
    computed: {
        parceiros: function () {
            return [];
        }
    },
    components: { PlaylistForm }
}
</script>
