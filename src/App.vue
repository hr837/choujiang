<template>
  <section class="page">
    <div class="component" ref="component">
      <div v-if="hasMsg" ref="message" class="message" :title="message">
        {{message}}
      </div>
    </div>
  </section>
</template>

<script lang="ts">
import { Component, Vue, Watch } from "vue-property-decorator";
import HelloWorld from "./components/HelloWorld.vue";

const MSG_LIST = [
  "测试文字移动",
  "测试😄😄😄😄😄😄😄文字移动",
  "这是一段比较长的文字，注意啦./components/HelloWorld.vue./components/HelloWorld.vue./components/HelloWorld." +
  "vue这是一段比较长的文字，注意啦./components/HelloWorld.vue./components/HelloWorld.vue./components/HelloWorld.vue",
  "三个字",
  "二〇一九〇一一四，这是一个无聊的测试例子。哈哈哈哈！！！！"
];

@Component({
  components: {
    HelloWorld
  }
})
export default class App extends Vue {

  private hasMsg = false;
  private message = "";
  private msgIndex = -1;
  private eventHandle = -1;

  @Watch("hasMsg")
  private onMsgFlagChange(val: boolean) {
    if (val) {
      this.msgIndex += 1;
      if (this.msgIndex === MSG_LIST.length) this.msgIndex = 0;
      this.message = MSG_LIST[this.msgIndex];
      this.$nextTick(this.getNext)
    } else {
      this.eventHandle = setTimeout(() => {
        this.hasMsg = true;
      }, 1);
    }
  }

  private getNext() {
    const message = this.$refs.message as HTMLElement;
    message.addEventListener("webkitAnimationEnd", () => {
      console.log(123)
      this.hasMsg = false
    })
  }



  private mounted() {
    window.clearTimeout(this.eventHandle);
    this.hasMsg = true;
  }
}
</script>

<style lang="less" scoped>
.page {
  height: 100%;
  width: 100%;
}

.component {
  height: 40px;
  line-height: 40px;
  width: 80%;
  margin: 20px auto;
  border: solid 1px gray;

  .message {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    // display: inline-block;
    animation: move 10s cubic-bezier(0, 1, 1, 1);
    &:hover {
      animation-play-state: paused;
    }
  }

  @keyframes move {
    from {
      background-color: pink;
      transform: translateX(100%);
    }
    to {
      background-color: yellow;
      transform: translateX(0);
    }
  }

  // .msg-enter-active {
  //   transition: all 10s ease;
  // }
  // .msg-enter-active,
  // .msg-leave-active {
  //   transition: all 10s linear;
  // }

  // .msg-enter {
  //   transform: translateX(100%);
  // }
  // .msg-leave-to {
  //   transform: translateX(0);
  // }
}
</style>

