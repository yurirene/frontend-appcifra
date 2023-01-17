<template >
  <div>
    <CirculoIdentificador :cor="parte.cor" :texto="parte.sigla" />
    <select v-model="partes[key].id">
      <option v-for="(p,k) in partesCifra" :key="`${key}_${k}`" v-bind:value="p.id">{{ p.titulo }}</option>
    </select>
  </div>
</template>
<script>
import CirculoIdentificador from './CirculoIdentificador.vue';

export default {
    components: { CirculoIdentificador }
}
</script>
<style >

</style>
