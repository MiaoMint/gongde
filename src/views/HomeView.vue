<script setup lang="ts">
import { ref, reactive } from "vue";
import gsap from 'gsap'
const itmes = ref<Array<number>>([])

const play = () => {
  const voice = new Audio('/mp3/muyu.mp3');
  voice.play();
  itmes.value.push(0);
  setTimeout(() => {
    itmes.value.shift()
  }, 500);
}


const onBeforeEnter = (el: { style: { opacity: number; height: number; }; }) => {
  el.style.opacity = 0
  el.style.height = 0
}

const onEnter = (el: { dataset: { index: number; }; }, done: any) => {
  gsap.to(el, {
    opacity: 1,
    height: '1.6em',
    delay: el.dataset.index * 0.15,
    onComplete: done
  })
}

const onLeave = (el: { dataset: { index: number; }; }, done: any) => {
  gsap.to(el, {
    opacity: 0,
    height: 0,
    delay: el.dataset.index * 0.15,
    onComplete: done
  })
}
</script>

<template>
  <main>
    <div class="plus">
      <TransitionGroup tag="ul" :css="false" @before-enter="onBeforeEnter" @enter="onEnter" @leave="onLeave">
        <li v-for="(item, index) in itmes" :key="index" :data-index="index">
          功德+1
        </li>
      </TransitionGroup>
    </div>
    <div class="muyu" @mousedown="play"></div>
  </main>
</template>

<style scoped lang="scss">
main {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;

  .plus {
    position: absolute;
    margin-bottom: 20rem;

    li {
      list-style: none;
    }
  }

  .muyu {
    cursor: url("/img/cursor.png") ,pointer;
    background-image: url("/img/muyu.png");
    background-position: center;
    background-repeat: no-repeat;
    background-size: 100%;
    width: 300px;
    height: 300px;
    transition: 1s cubic-bezier(0.075, 0.82, 0.165, 1);

    &:hover {
      transform: scale(1.4)
    }

    &:active {
      transform: scale(0.4)
    }
  }

}
</style>
