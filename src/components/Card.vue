<template>
  <div id="card">
    <span id="value-label">{{value}}</span>
  </div>
</template>

<script>
export default {
  name: 'Card',
  props: {
    value: String
  }
}
</script>

<style scoped>
  #card {
    border: 1px solid lightblue;
    background-color: beige;
    justify-content: center;
    align-items: center;
    display: flex;
    width: 114px;
    height: 176px;
  }

  #value-label {
    font-size: 32px;
    font-weight: bold;
  }
</style>
