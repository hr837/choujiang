<template>
  <section class="component-poker">
    <div class="poker-container" :class="{red:red,done:done}">
      <div class="left">
        <div>{{pokerNumber}}</div>
        <div class="both-end-poker-type">{{pokerType}}</div>
      </div>
      <div class="content">
        <div v-if="particularPoker">
          {{pokerNumber}}
        </div>
        <div v-else class="poker-type-list">
          <span class="poker-type" v-for="item of normalNumber" :key="item">{{pokerType}}</span>
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
    return this.pokerStr[1]
  }

  private get normalNumber() {
    if (!this.pokerStr || this.particularPoker) return 0
    return Number.parseInt(this.pokerStr[1], undefined)
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
      padding: 10px 5px;
    }
  }
  .poker-type-list {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    align-content: stretch;
    .poker-type {
      font-size: 40px;
      width: 60px;
      height: 60px;
    }
  }
}
</style>
