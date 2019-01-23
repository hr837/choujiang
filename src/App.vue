<template>
  <section class="page">
    <div class="top">指旺金科西安中心 2019 年年会抽奖活动</div>
    <div class="container">
      <div class="left">
        <button class="flip red-button" @click="flipClick">打乱顺序</button>
        <transition-group name="flip" tag="ul" class="poker-list">
          <li class="poker-list-item" v-for="(item,index) of pokerList" :key="item" :class="{red: ['♥','♦'].includes(item[0])}">
            <span>{{item}}</span> <a @click="remove(index)" class="remove">X</a>
          </li>
        </transition-group>
        <div>共计：{{pokerList.length}}</div>
      </div>
      <div class="content">
        <div class="history-title" v-if="valueHistory.length">
          <text-keep-moving :msgList="valueHistory"></text-keep-moving>
        </div>
        <poker class="poker" :class="{done:done}" :done="done" v-show="value" :pokerStr="value"></poker>
        <div class="operate">
          <button @click="startClick" v-if="done" class="start red-button">开始</button>
          <button @click="stop" ref="stop" v-else class="start red-button">停止</button>
        </div>
      </div>
    </div>

  </section>
</template>

<script lang="ts">
import { Component, Vue, Watch } from "vue-property-decorator";
import Poker from "./components/poker.vue";
import TextKeepMoving from "./components/text-keep-moving.vue";

const SOURCE = [
  "♠1", "♠2", "♠3", "♠4", "♠5", "♠6", "♠7", "♠8", "♠9", "♠10", "♠J", "♠Q", "♠K",
  "♥1", "♥2", "♥3", "♥4", "♥5", "♥6", "♥7", "♥8", "♥9", "♥10", "♥J", "♥Q", "♥K",
  "♣1", "♣2", "♣3", "♣4", "♣5", "♣6", "♣7", "♣8", "♣9", "♣10", "♣J", "♣Q", "♣K",
  "♦1", "♦2", "♦3", "♦4", "♦5", "♦6", "♦7", "♦8", "♦9", "♦10", "♦J", "♦Q", "♦K",
]
const MaxStopTimmer = 2000;

@Component({
  components: {
    Poker,
    TextKeepMoving
  }
})
export default class App extends Vue {

  private pokerList: string[] = []
  private value = "";
  private handlerNumber = -1;

  private done = true;
  private valueHistory: string[] = []

  @Watch("done")
  private onDoneChange() {
    if (!this.done) return
    this.valueHistory.push(this.value)
    const index = this.pokerList.indexOf(this.value)
    this.pokerList.splice(index, 1)
  }

  private mounted() {
    this.pokerList = [...SOURCE]
  }

  private startClick() {
    clearTimeout(this.handlerNumber)
    this.valueHistory.splice(0, 1)
    this.done = false
    this.createNewPoker()

    this.handlerNumber = setInterval(this.createNewPoker, 50);

  }

  private stop() {
    clearInterval(this.handlerNumber)
    this.done = true
  }
  private createNewPoker() {
    const stopEl = this.$refs.stop as HTMLElement
    if (stopEl) {
      stopEl.focus()
    }
    this.value = this.pokerList[this.getRandom()]
  }

  private getRandom() {
    let random = Math.round(Math.random() * 100)
    if (random >= this.pokerList.length) {
      random = this.getRandom()
    }
    return random;
  }

  private flipClick() {
    this.pokerList.sort(() => 0.5 - Math.random())
  }

  private remove(index: number) {
    this.pokerList.splice(index, 1)
  }

}
</script>

<style lang="less" scoped>
.page {
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: column;
  .top {
    flex-basis: 90px;
    text-align: center;
    font-size: 60px;

    background-image: -webkit-linear-gradient(
      right,
      #b60000,
      #27ff00 25%,
      #ff00f7 50%,
      #0066ff 75%,
      #b60000
    );
    -webkit-text-fill-color: transparent;
    -webkit-background-clip: text;
    -webkit-background-size: 200% 100%;
    -webkit-animation: masked-animation 4s linear infinite;

    @keyframes masked-animation {
      0% {
        background-position: 0 0;
      }
      100% {
        background-position: -100% 0;
      }
    }
  }
  .container {
    flex: 1;
    display: flex;
    .left {
      flex-basis: 100px;
    }
    .content {
      flex: 1;
    }
  }

  .content {
    position: static;
    .operate {
      position: absolute;
      right: 50px;
      bottom: 50px;
      .start {
        width: 90px;
        height: 90px;
        border-radius: 45px;
        &:hover {
          animation: start 1.5s linear infinite;
        }
        &:focus {
          outline: none;
        }

        @keyframes start {
          0% {
            font-size: 14px;
            border-radius: 45px;
          }
          50% {
            font-size: 24px;
            border-radius: 30px;
          }
          100% {
            font-size: 14px;
            border-radius: 45px;
          }
        }
      }
    }
    .poker {
      margin-top: 100px;
      @rotate: 360deg;
      &.done {
        animation: done 5s ease-out;
      }
      @keyframes done {
        0% {
          transform: rotate3d(1, 0, 0, 0deg);
        }
        10% {
          transform: rotate3d(1, 1, 0, 180deg);
        }
        20% {
          transform: rotate3d(1, 1, 1, 45deg);
        }
        30% {
          transform: rotateX(180deg);
        }
        40% {
          transform: rotate3d(0, 1, 0, 90deg);
        }
        50% {
          transform: rotate3d(0, 0, -2, 140deg);
        }
        60% {
          transform: rotateY(180deg);
        }
        70% {
          transform: rotate3d(0.8, 1, 0, 180deg);
        }
        80% {
          transform: rotate3d(0.6, 0.3, 0, 270deg);
        }
        90% {
          transform: rotate3d(0.2, -0.6, 0.5, 180deg);
        }
        100% {
          transform: rotate3d(1, 0, 1, 360deg);
        }
      }
      .title {
        font-size: 30px;
      }
    }
    .history-title {
      height: 40px;
      font-size: 16px;
    }
  }

  .left {
    padding-left: 20px;
    padding-top: 50px;
    .poker-list {
      max-height: 400px;
      overflow-y: auto;
      margin-top: 10px;
      padding: 0;
      list-style: none;
      .poker-list-item + .poker-list-item {
        margin: 2px 0;
      }
      &-item {
        line-height: 26px;
        background-color: #fdfdfd;
        padding-left: 20px;
        display: flex;
        justify-content: space-between;
        align-items: center;
        &:hover {
          background-color: #f2f2f2;
          .remove {
            display: unset;
          }
        }
        .remove {
          color: white;
          width: 16px;
          height: 16px;
          border-radius: 8px;
          background-color: #b60000;
          margin-right: 5px;
          font-size: 13px;
          line-height: 16px;
          text-indent: 4px;
          display: none;
        }
      }
    }
    .flip {
      height: 40px;
      width: 100px;
    }
  }

  .flip-move {
    transition: transform 1s;
  }
  .flip-enter-active,
  .flip-leave-active {
    transition: all 1s;
  }
  .flip-enter,
  .flip-leave-to {
    opacity: 0;
    transform: translateX(90px);
  }
}
</style>

<style lang="less">
.page {
  .red {
    color: red;
  }
  button,
  a {
    &:hover {
      cursor: pointer;
    }
  }
  .red-button {
    border-color: #ffffff;
    background-color: #b60000;
    border: 0;
    color: white;
    font-size: 16px;
    &:focus {
      outline-color: #b60000;
    }
  }
}
</style>


