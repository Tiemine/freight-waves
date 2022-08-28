<template>
  <div class="modal">
    <div class="modal__text">Loading Users</div>
    <div class="modal__background"></div>
  </div>
</template>

<script>
export default {
  name: 'Modal'
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.modal{
  width: 100vw;
  height: 100vh;
  position: fixed;
  left: 0;
  top: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  &__background{
    background: white;
    opacity: 0.7;
    height: 100%;
    width: 100%;
    position: absolute;
    top: 0;
    z-index: 1;
  }
  &__text{
    position: relative;
    font-size: 20px;
    z-index: 2;
    &:after{
      content: '';
      box-sizing: border-box;
      width: 35px;
      height: 35px;
      position: absolute;
      top: calc(50% - 20px);
      right: -40px;
      border-radius: 50%;
      border: 2px solid rgb(0, 153, 255);
      border-right: 2px solid rgba(255, 255, 255, 0.0);
      animation: spinner .5s linear infinite;
    }
    @keyframes spinner {
      to {transform: rotate(360deg);}
    }
  }
}
</style>
