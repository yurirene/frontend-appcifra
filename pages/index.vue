<template>
  <div>
    <div class="page-content">


      <div class="content">
        <div class="row mb-n2">
          <div class="col-12 pe-2">
            <div class="card card-style mx-0 mb-3">
              <div class="p-3">
                <h4 class="font-700 text-uppercase font-12 opacity-50 mt-n2">Cifras Cadastradas</h4>
                <h1 class="font-700 font-34 color-blue-dark mb-0">AAA</h1>
                <i class="fa fa-music float-end mt-n3 opacity-40"></i>
              </div>
            </div>
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

    }
  },
  mounted() {
    this.$loadScript("/assets/scripts/custom.js")
    .then(()=> {init_template();})
  },
}
</script>
