<template>
  <div>
    <div class="page-content">

    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {

    }
  },
  mounted() {
    this.$loadScript("/assets/scripts/custom.js")
    .then(()=> {init_template();})
  },
}
</script>
