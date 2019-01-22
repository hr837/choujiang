<template>
  <section class="page">
    <div class="top">指旺金科西安中心 2019 年年会抽奖活动</div>
    <div class="container">
      <div class="left">
        <button class="flip" @click="flipClick">打乱顺序</button>
        <transition-group name="flip" tag="ul" class="poker-list">
          <li class="poker-list-item" v-for="(item,index) of pokerList" :key="item" :class="{red: ['♥','♦'].includes(item[0])}">
            <span>{{item}}</span> <a @click="remove(index)" class="remove">X</a>
          </li>
        </transition-group>
        <div>共计：{{pokerList.length}}</div>
      </div>
      <div class="content">
        <div class="operate">
          <button @click="startClick" v-if="done" class="start">开始</button>
        </div>
        <poker class="poker" :class="{done:done}" :done="done" v-show="value" :pokerStr="value"></poker>

        <div v-if="valueHistory.length">中将历史:{{valueHistory}}</div>
      </div>
    </div>

  </section>
</template>

<script lang="ts">
import { Component, Vue, Watch } from "vue-property-decorator";
import Poker from "./components/poker.vue";

const SOURCE = [
  "♠1", "♠2", "♠3", "♠4", "♠5", "♠6", "♠7", "♠8", "♠9", "♠10", "♠J", "♠Q", "♠K",
  "♥1", "♥2", "♥3", "♥4", "♥5", "♥6", "♥7", "♥8", "♥9", "♥10", "♥J", "♥Q", "♥K",
  "♣1", "♣2", "♣3", "♣4", "♣5", "♣6", "♣7", "♣8", "♣9", "♣10", "♣J", "♣Q", "♣K",
  "♦1", "♦2", "♦3", "♦4", "♦5", "♦6", "♦7", "♦8", "♦9", "♦10", "♦J", "♦Q", "♦K",
]
const MaxStopTimmer = 2000;

@Component({
  components: {
    Poker
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
  }

  private mounted() {
    this.pokerList = [...SOURCE]
  }

  private startClick() {
    clearTimeout(this.handlerNumber)
    this.done = false
    this.createNewPoker()

    const startHandle = setInterval(this.createNewPoker, 50);

    this.handlerNumber = setTimeout(() => {
      clearInterval(startHandle)
      this.stop()
    }, 5000);
  }

  private stop() {

    // 停止之前的interval
    window.clearInterval(this.handlerNumber)

    let handler = -1;
    const getNext = (timmer: number) => {
      clearTimeout(handler)
      if (timmer < MaxStopTimmer) {
        this.createNewPoker()
        handler = setTimeout(() => {
          getNext(timmer += 300)
        }, timmer);
      } else {
        this.done = true
      }
    }
    getNext(100)
  }
  private createNewPoker() {
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
  border: solid 4px #f2f2f2;
  display: flex;
  flex-direction: column;
  .top {
    flex-basis: 60px;
    text-align: center;
    font-size: 40px;
    background-color: aquamarine;
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
    .operate {
      text-align: right;
      margin-right: 50px;
      > button {
        &:hover {
          cursor: pointer;
        }
      }
      .start {
        width: 90px;
        height: 90px;
        background-color: yellowgreen;
        border-radius: 45px;
        color: white;
        border: 0;
        &:hover {
          animation: start 2s linear infinite;
        }

        @keyframes start {
          0% {
            font-size: 14px;
            background-color: green;
          }
          50% {
            font-size: 24px;
            background-color: red;
          }
          100% {
            font-size: 14px;
            background-color: green;
          }
        }
      }
    }
    .poker {
      @rotate: 360deg;
      &.done {
        animation: done 3s ease-out;
      }
      @keyframes done {
        0% {
          transform: rotate(@rotate);
        }
        25% {
          transform: rotate(@rotate*2);
        }
        50% {
          transform: rotate(@rotate*3);
        }
        100% {
          transform: rotate(@rotate*4);
        }
      }
      .title {
        font-size: 30px;
      }
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
        background-color: #f2f2f2;
        display: flex;
        justify-content: space-between;
        align-items: center;
        &:hover {
          .remove {
            display: unset;
          }
        }
        .remove {
          color: white;
          width: 16px;
          height: 16px;
          border-radius: 8px;
          background-color: red;
          margin-right: 5px;
          font-size: 13px;
          line-height: 16px;
          text-indent: 4px;
          display: none;
        }
      }
    }
    .flip {
      border-color: #409eff;
      height: 30px;
      width: 90px;
      background-color: #409eff;
      border-radius: 5px;
      border: 0;
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
}
</style>


