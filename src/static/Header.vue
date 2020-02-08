<template>
  <header class="header">
    <div class="header__inner">
      <h1>UK Police Crime Map</h1>
      <p>Search any UK Postcode to get the recorded crimes in that area</p>
    </div>
  </header>
</template>

<script>
export default {
  name: 'Header',
};
</script>

<style lang="scss" scoped>
.header {
  width:100%;
  background: #323D69;
  display: flex;
  justify-content: center;
  color:#fff;
  &__inner {
    width:100%;
    padding:30px 0;
    text-align: center;
    max-width: 1280px;
    margin: 0 15px;

    h1 {
      font-size: 3em;

      @media(max-width: 600px) {
        font-size: 2em;
      }
    }
  }
}
</style>
