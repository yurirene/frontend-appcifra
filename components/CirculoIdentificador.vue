<template>
  <div>
    <span
      :style="`
        margin-left: 0px;
        border: 2px solid black;
        border-color: ${cor};
        font-weight: 800;
        border-radius: 50%;
        width: 25px;
        height: 25px;
        display:inline-flex;
        justify-content:center;
        align-items:center;
        float:left;
    `">
      <span style="font-size: 12px;">{{ texto }}</span>
    </span>
  </div>
</template>
<script>
export default {
  props: {
    cor: {
      type: String,
      default: '#eee'
    },
    texto: {
      type: String
    }
  }
}
</script>
