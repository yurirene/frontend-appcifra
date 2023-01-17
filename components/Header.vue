<template>
  <div>

    <div class="header header-fixed header-logo-center">
        <a href="#" class="header-title">IPC Cifras</a>
    </div>
    <div class="row mt-3"></div>
    <div class="card mt-5 card-style preload-img bg-0-index" data-card-height="90" style="background-image: url(/assets/images/18w.jpg);" v-if="cardShow">
        <div class="card-center ms-3">
          <h1 class="color-white mb-0">App</h1>
          <p class="color-white mt-n1 mb-0">App de Cifras Calvário</p>
        </div>
        <div class="card-overlay bg-black opacity-80"></div>
      </div>
  </div>
</template>
<script>
export default {
  props: {
    cardShow: {
      type: Boolean,
      default: true
    }
  },

}
</script>
<style>

</style>
