<script setup lang="ts">
import { computed, ref, watch } from "vue";
const itmes = ref<Array<number>>([])
const count = ref<number>()

count.value = Number(window.localStorage.getItem("gongde"))

const play = () => {
  window.localStorage.setItem("gongde", String(++count.value!))
  const voice = new Audio('/mp3/muyu.mp3');
  voice.play();
  itmes.value.push(Math.random());

  setTimeout(() => {
    itmes.value.shift()
  }, 500);
}

</script>

<template>
  <main>
    <div class="plus">
      <TransitionGroup tag="ul" name="list">
        <li style="color: #e6b422" v-for="(item, index) in itmes" :key="item" :data-index="index">
          功德+1
        </li>
      </TransitionGroup>
    </div>
    <div class="muyu" @mousedown="play"></div>
    <div style="color: #e6b422;font-weight: bold;">
      功德：{{ count }}
    </div>
  </main>
</template>

<style scoped lang="scss">
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
}

main {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  flex-direction: column;

  .plus {
    position: absolute;
    margin-bottom: 20rem;

    ul {
      padding: 0;
      margin: 0;
    }

    li {
      list-style: none;
    }
  }

  .muyu {
    outline: none;
    -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
    cursor: url("/img/cursor.png"), pointer;
    background-image: url("/img/muyu.png");
    background-position: center;
    background-repeat: no-repeat;
    background-size: 100%;
    width: 300px;
    height: 300px;
    transition: .5s cubic-bezier(0.075, 0.82, 0.165, 1);

    &:hover {
      transform: scale(1.2)
    }

    &:active {
      transform: scale(0.6)
    }
  }

}

</style>
