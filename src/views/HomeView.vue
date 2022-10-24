<script setup lang="ts">
</script>

<template>
  <main>
    <div class="muyu">

    </div>
  </main>
</template>

<style scoped lang="scss">
main {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;

  .muyu {
    background-image: url("/img/muyu.png");
    background-position: center;
    background-repeat: no-repeat;
    background-size: 100%;
    width: 200px;
    height: 200px;
    transition: 1s cubic-bezier(0.075, 0.82, 0.165, 1);

    &:hover {
      transform: scale(1.4)
    }

    &:active{
      transform: scale(0.4)
    }
  }

}
</style>
