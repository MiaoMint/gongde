<script setup lang="ts">
import { ref } from "vue";
const items = ref<Array<{ id: number; text: String }>>([]);
const count = ref<number>(Number(localStorage.getItem("gongde")));
const fozuText = ref<string>("扣1佛祖陪你笑");
const isPopup = chrome.extension !== undefined;

const danmaku = (text: string) => {
  items.value.push({ id: Math.random(), text });
  setTimeout(() => {
    items.value.shift();
  }, 300);
};

const play = () => {
  const muyuVoice = new Audio("/mp3/muyu.ogg");
  localStorage.setItem("gongde", String(++count.value));
  muyuVoice.play();
  danmaku("功德+1");
};

const laugh = () => {
  const fozuVoice = new Audio("/mp3/fozu.ogg");
  if (Number(localStorage.getItem("gongde")) >= 10) {
    count.value -= 10;
    localStorage.setItem("gongde", String(count.value));
    danmaku("功德-10");
    fozuVoice.play();
  } else {
    fozuText.value = "功德不足";
    setTimeout(() => (fozuText.value = "扣1佛祖陪你笑"), 2000);
  }
};

document.addEventListener("keypress", (event) => {
  if (event.code === "Digit1") laugh();
});
</script>

<template>
  <main :class="{ popup: isPopup }">
    <div class="plus">
      <TransitionGroup tag="ul" name="list">
        <li
          style="color: #e6b422"
          v-for="(item, index) in items"
          :key="item.id"
          :data-index="index"
        >
          {{ item.text }}
        </li>
      </TransitionGroup>
    </div>
    <div class="muyu" @mousedown="play"></div>
    <div class="text">功德：{{ count }}</div>
    <div class="text" @mousedown="laugh">
      {{ fozuText }}
    </div>
  </main>
</template>

<style scoped lang="scss">
.popup {
  width: 300px;
  height: 500px;
}

.list-enter-active,
.list-leave-active {
  transition: all 0.3s ease;
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
    transition: 0.5s cubic-bezier(0.075, 0.82, 0.165, 1);

    &:hover {
      transform: scale(1.2);
    }

    &:active {
      transform: scale(0.6);
    }
  }

  .text {
    color: #e6b422;
    font-weight: bold;
    margin-bottom: 1em;
  }
}
</style>
