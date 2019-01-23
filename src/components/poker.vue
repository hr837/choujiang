<template>
  <section class="component-poker">
    <div class="poker-container" :class="{red:red,done:done}">
      <div class="left">
        <div>{{pokerNumber}}</div>
        <div class="both-end-poker-type">{{pokerType}}</div>
      </div>
      <div class="content">
        <div v-if="particularPoker" class="particularPoker">
          <div></div>
        </div>
        <div v-else class="poker-type-list" :class="`item-count-${normalNumber}`">
          <div class="poker-type" :class="`poker-item-${item}`" v-for="item of normalNumber" :key="item">{{pokerType}}</div>
        </div>
      </div>
      <div class="right">
        <div>{{pokerNumber}}</div>
        <div class="both-end-poker-type">{{pokerType}}</div>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component({})
export default class Poker extends Vue {
  @Prop({ type: String })
  private pokerStr!: string;

  @Prop({ type: Boolean })
  private done!: boolean

  private get pokerNumber() {
    if (!this.pokerStr) return ""
    return this.pokerStr.substring(1)
  }

  private get normalNumber() {
    if (!this.pokerStr || this.particularPoker) return 0
    return Number.parseInt(this.pokerStr.substring(1), undefined)
  }

  private get particularPoker() {
    return ["J", "Q", "K"].includes(this.pokerNumber)
  }

  private get pokerType() {
    if (!this.pokerStr) return ""
    return this.pokerStr[0]
  }

  private get red() {
    return ["♥", "♦"].includes(this.pokerType)
  }


}
</script>

<style lang="less" scoped>
.component-poker {
  text-align: center;
  .poker-container {
    height: 8.7cm;
    width: 5.7cm;
    border: solid 1px #f2f2f2;
    border-radius: 10px;
    display: flex;
    box-shadow: 1px 1px 11px 1px #bebebe;
    margin: 10px auto;
    padding: 15px 10px;

    .both-end-poker-type {
      font-size: 24px;
      line-height: 16px;
    }
    .left,
    .right {
      flex-basis: 30px;
      font-size: 24px;
    }
    .right {
      transform: rotate(180deg);
    }
    .content {
      flex: 1;
      padding: 30px 5px;
    }
  }

  .particularPoker {
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .poker-type-list {
    height: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    .poker-type {
      font-size: 40px;
      width: 50px;
      display: table;
    }
  }
  .item-count-1 {
    justify-content: center;
    .poker-item-1 {
      font-size: 100px;
    }
  }
  .item-count-2,
  .item-count-3 {
    .poker-item-1 {
      transform: rotate(180deg);
    }
    flex-direction: column;
    justify-content: space-around;
  }

  .item-count-4,
  .item-count-6 {
    .poker-item-1,
    .poker-item-2 {
      transform: rotate(180deg);
    }
  }
  .item-count-5 {
    justify-content: center;
    .poker-item-1,
    .poker-item-2 {
      transform: rotate(180deg);
    }
    .poker-item-3 {
      width: 100%;
    }
  }

  .item-count-7 {
    flex-direction: column;
    .poker-type {
      flex-basis: 33.333%;
    }
    .poker-item-4 {
      flex-basis: 100%;
      padding-bottom: 25%;
    }
    .poker-item-1,
    .poker-item-4,
    .poker-item-5 {
      transform: rotate(180deg);
    }
  }
  .item-count-8 {
    flex-direction: column;
    .poker-type {
      flex-basis: 33.333%;
    }
    .poker-item-4,
    .poker-item-5 {
      flex-basis: 50%;
      padding-bottom: 25%;
    }
    .poker-item-1,
    .poker-item-4,
    .poker-item-6 {
      transform: rotate(180deg);
    }
  }
  .item-count-9 {
    flex-direction: column;
    .poker-type {
      flex-basis: 33.333%;
    }
    .poker-item-1,
    .poker-item-4,
    .poker-item-7 {
      transform: rotate(180deg);
    }
  }
  .item-count-10 {
    flex-direction: column;
    .poker-type {
      flex-basis: 25%;
    }
    .poker-item-5,
    .poker-item-6 {
      flex-basis: 50%;
      padding-bottom: 25%;
    }
    .poker-item-1,
    .poker-item-2,
    .poker-item-5,
    .poker-item-7,
    .poker-item-8 {
      transform: rotate(180deg);
    }
  }
}
</style>
