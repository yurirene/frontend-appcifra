<template>
  <div>
    <div class="page-content">
      <div class="card card-style">
        <div class="content">
          <div class="row">
            <div class="col">
              <h1>Editor da Cifra</h1>
              <p><em>{{ musica.titulo }}</em></p>
            </div>

            <div class="col">
              <a href="/cadastro"
                data-back-button class="btn btn-sm float-end rounded-xl shadow-xl text-uppercase font-800 bg-highlight"
              >
                <em class="fas fa-arrow-left ms-1"></em> Voltar
              </a>
            </div>
          </div>
        </div>
      </div>

      <div class="card card-style">
        <div class="content">
          <div class="row">
            <div class="col-6">
              <h1>Seção</h1>
              <p><em>Partes da Música</em></p>
              <div class="row">
                <div class="col-6">
                  <select v-model="idNovaSecao" class="form-control">
                    <option v-for="(p,k) in partesCifra" :key="k" :value="p.id">
                      {{ p.titulo }}
                    </option>
                  </select>
                </div>
                <div class="col">
                  <button class="btn btn-primary rounded-m" @click="novaSecao">
                    <em class="fas fa-plus"></em>
                    Seção
                  </button>
                </div>
              </div>
            </div>
            <div class="col-6 border-left">
              <h1>Ordem</h1>
              <div class="row">
                <div class="col">
                  <span
                  @click="removerDaOrdem(k)"
                    v-for="(o,k) in ordemDefinida"
                    :key="k"
                  >
                    <CirculoIdentificador
                      :cor="o.cor"
                      :texto="o.sigla"
                    />
                  </span>
                </div>
              </div>
              <div class="row">
                <div class="col-6">
                  <input type="text" class="form-control" v-model="ordemTexto" disabled />
                </div>
                <div class="col">
                  <span @click="adicionarNaOrdem(k)"
                    v-for="(p,k) in partesDisponiveis"
                    :key="k"
                  >
                    <CirculoIdentificador
                    :cor="p.cor"
                    :texto="p.sigla"
                    />
                  </span>
                </div>
              </div>
              <div class="row">
                <div class="col">
                  <button class="btn btn-primary rounded-m" @click="atualizarOrdem">
                    <em class="fas fa-save"></em>
                    Ordem
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="card card-style" v-for="(parte, key) in partes" :key="key" :style="`border-left: 3px solid ${parte.cor} !important; `" >
        <div class="content mb-2">
          <div class="row d-flex align-items-center">
            <div class="col">
              <p>
                <CirculoIdentificador :cor="parte.cor" :texto="parte.sigla" />
                <span class="ms-2">{{ parte.titulo }}</span>
              </p>
            </div>
            <div class="col  d-flex">
              <button class="btn ms-auto" @click="atualizarSecao(key)">
                <em class="fas fa-save text-success"></em>
              </button>
              <button class="btn " @click="removerSecao(key)">
                <em class="fas fa-trash text-danger"></em>
              </button>
            </div>
          </div>

          <client-only>
            <editor-cifra v-model="partes[key].cifra" />
          </client-only>
        </div>
      </div>


    </div>
  </div>
</template>
<script>

import EditorCifra from '~/components/EditorCifra.vue'
import CirculoIdentificador from '../../components/CirculoIdentificador.vue';
export default {
  components: {
    EditorCifra,
    CirculoIdentificador
},
  data() {
    return {
      idNovaSecao: null,
      partesCifra: [],
      musica: {},
      ordem: [],
      partes: []
    }
  },
  mounted() {
    this.$loadScript("/assets/scripts/custom.js")
    .then(()=> {init_template();})

    this.carregarPartesCifras();
    this.carregarPartes();
  },
  computed: {
    partesDisponiveis: function(){
      let partes = this.partes.map((item) => { return item.id});
      return this.partesCifra.filter((item) => {
        return partes.includes(item.id);
      });
    },
    ordemDefinida: {
      get: function() {
        let ordem = [];
        if (!this.ordem) {
          return [];
        }
        this.ordem.forEach((item) => {
          let escolha = this.partesDisponiveis.find((parte) => {
            return parte.sigla == item;
          });
          ordem.push(escolha);
        })
        return ordem;
      },
      set: function(value) {
        let ordem = [];
        value.forEach((item) => {
          ordem.push(item.sigla);
        })
        this.ordem = ordem;
      }
    },
    ordemTexto: function() {
      if (!this.ordem) {
        return '';
      }
      return this.ordem.join(',');
    }
  },
  methods: {
    carregarPartesCifras: function() {
      this.$axios.$get('/partes')
      .then((response) => {
        this.partesCifra = response;
      });
    },
    carregarPartes: function() {
      this.$axios.$get('/musicas/' + this.$route.params.id)
      .then((response) => {
        this.musica = response.musica;
        this.ordem = response.ordem;
        this.partes = response.partes.map((item) => {
          return {
            id: item.id,
            titulo: item.titulo,
            sigla: item.sigla,
            cifra: item.pivot.cifra,
            cor: item.cor,
            secao: {
              musicaId: item.pivot.musica_id,
              parteId: item.pivot.parte_id
            }
          }
        });
      });
    },
    removerSecao: function(key) {
      let parte = this.partes[key];
      this.$axios.$post('/musicas/remover-secao', {
        parte_id: parte.secao.parteId,
        musica_id: this.musica.id
      })
      .then((response) => {
        this.carregarPartes();
      })
    },
    novaSecao: function() {
      let parte = this.partes[0];
      this.$axios.$post('/musicas/nova-secao', {
        parte_id: this.idNovaSecao,
        musica_id: this.musica.id
      })
      .then((response) => {
        this.carregarPartes();
      })
    },
    atualizarSecao: function(key) {
      let parte = this.partes[key];
      this.$axios.$post('/musicas/atualizar-secao', {
        parte_id: parte.secao.parteId,
        musica_id: this.musica.id,
        cifra: parte.cifra
      })
      .then((response) => {
        this.carregarPartes();
      })
    },
    adicionarNaOrdem: function(key) {
      let parte = this.partesDisponiveis[key];
      this.ordem.push(parte.sigla);
    },
    removerDaOrdem: function(key) {
      let ordem = this.ordemDefinida;
      ordem.splice(key, 1);
      this.ordemDefinida = ordem;
    },
    atualizarOrdem: function() {
      this.$axios.$post('/musicas/atualizar-ordem', {
        musica_id: this.musica.id,
        ordem: this.ordem
      });
    }
  }
}
</script>
