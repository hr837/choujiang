<template>
  <div class="component" ref="component">
    <div v-if="hasMsg" ref="message" class="message" :class="{red: ['♦','♥'].includes(message[0])}" :title="message">
      {{message}}
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Watch, Prop } from "vue-property-decorator";

@Component({})
export default class TextKeepMoving extends Vue {

  @Prop({ default: () => [] })
  private msgList: string[];

  private hasMsg = false;
  private message = "";
  private msgIndex = -1;
  private eventHandle = -1;

  @Watch("hasMsg")
  private onMsgFlagChange(val: boolean) {
    if (val) {
      this.msgIndex += 1;
      if (this.msgIndex === this.msgList.length) this.msgIndex = 0;
      this.message = this.msgList[this.msgIndex];
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

  .message {
    white-space: nowrap;
    overflow: hidden;
    font-size: 16px;
    text-overflow: ellipsis;
    animation: move 10s linear;
    &:hover {
      animation-play-state: paused;
    }
  }

  @keyframes move {
    from {
      transform: translateX(100%);
    }
    to {
      transform: translateX(0);
    }
  }
}
</style>

