<template>
  <div class="loading">
    <div class="loading__inner">
      <div class="loading__inner-inner">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Loading',
};
</script>

<style lang="scss" scoped>
.loading {
  margin: 50px 0;
  width:70px;
  height: 70px;
  border-radius: 50%;
  border-left: 5px solid #323D69;
  border-top: 5px solid rgba($color: #323D69, $alpha: 0.3);
  border-right: 5px solid rgba($color: #323D69, $alpha: 0.3);
  border-bottom: 5px solid rgba($color: #323D69, $alpha: 0.3);
  transform: rotate(0deg);
  animation: spin 1s forwards infinite linear;

  &__inner {
    width:60px;
    height: 60px;
    border-radius: 50%;
    border-left: 5px solid #323D69;
    border-top: 5px solid rgba($color: #323D69, $alpha: 0.5);
    border-right: 5px solid rgba($color: #323D69, $alpha: 0.5);
    border-bottom: 5px solid rgba($color: #323D69, $alpha: 0.5);
    transform: rotate(0deg);
    animation: spin 2.5s forwards infinite linear;

    &-inner {
      width:50px;
      height: 50px;
      border-radius: 50%;
      border-left: 5px solid #323D69;
      border-top: 5px solid rgba($color: #323D69, $alpha: 0.7);
      border-right: 5px solid rgba($color: #323D69, $alpha: 0.7);;
      border-bottom: 5px solid rgba($color: #323D69, $alpha: 0.7);
      transform: rotate(0deg);
      animation: spin 5s forwards infinite linear;
    }
  }
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}
</style>
