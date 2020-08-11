<template>
  <div
    class="gee-mark-digitalscroll"
    :style="{
      height:fontSize+'px',
      lineHeight:fontSize+'px'
    }"
  >
    <p
      v-for="(item,index) in computeNumber"
      :key="index"
      class="gee-mark-digitalscroll__item"
      :style="{
        width:(fontSize/2+5)+'px',
        fontSize:fontSize+'px'
      }"
    >
      <span
        ref="numberDom"
        :style="{
          color:color,
          letterSpacing:fontSize+'px'
        }"
      >0123456789</span>
    </p>
    <div :style="{
      width:(fontSize/2+5)+'px', 
      color:color,
      fontSize:fontSize+'px'}"
    >
      {{suffix}}
    </div>
  </div>
</template>
<script lang="ts">
import { Vue, Component, Prop, Watch } from 'vue-property-decorator'

@Component({
  name: 'numberScroll'
})
export default class NumberScroll extends Vue {
  @Prop({
    type: Number,
    default: 0,
    required: true
  })
  public number!: number

  @Prop({
    type: String,
    default: '#000000'
  })
  public color!: string

  @Prop({
    type: [String, Number],
    default: 12
  })
  public fontSize!: string | number

  @Prop({
    type: String,
    default: '+'
  })
  public suffix!: string

  @Prop({
    type: Boolean,
    default: false
  })
  public isSelfIncreasing!: boolean

  @Watch('number')
  public numberChange(val: any) {
    this.newNumber = val
    this.refresh()
  }

  private maxLen: number = 8
  private computeNumber: string[] = []
  private timeTicket: any = null
  private newNumber = this.number

  public mounted() {
    this.isSelfIncreasing ? this.increaseNumber() : this.refresh()
  }

  public destroyed() {
    this.clearTimeOut()
  }

  /**
   * @description: 数字补零操作，返回数组
   * @param {number} num 被操作数
   * @param {number} n 总长度
   * @return: string[]
   */
  private prefixZero(num: number, n: number) {
    return (Array(n).join('') + num).slice(-n).split('')
  }

  /**
   * @description: 获取随机数
   * @param {type}
   * @return: number
   */
  private getRandomNumber(min: number, max: number) {
    return Math.floor(Math.random() * (max - min + 1) + min)
  }

  /**
   * @description: 设置每一位数字的偏移
   */
  private setNumberTransform() {
      const numberItems: any = this.$refs.numberDom
      for (let index = 0; index < numberItems.length; index++) {
        const elem = numberItems[index]
        elem.style.transform = `translate(-50%,-${Number(
          this.computeNumber[index]
        ) * 10}%`
      }
  }

  /**
   * @description: 刷新数据
   * @param {number} randomNumber 自增随机字符
   */
  private async refresh(randomNumber?: number) {
    this.computeNumber = randomNumber ?
    (() => {
      this.newNumber = this.newNumber + randomNumber
      return this.prefixZero(this.newNumber, this.maxLen)
    })() :
    this.prefixZero(this.newNumber, this.maxLen)
    const t = setTimeout(() => {
      this.setNumberTransform()
      clearTimeout(t)
    })
  }

  /**
   * @description: 随机增加数量
   * @param {number} randomNumber 自增随机字符
   */
  private increaseNumber(randomNumber?: number) {
    this.clearTimeOut()
    randomNumber ? this.refresh(randomNumber) : this.refresh()
    this.timeTicket = setTimeout(() => {
      const randomNum = this.getRandomNumber(1, 20)
      this.increaseNumber(randomNum)
    }, 3000)
  }

  /**
   * @description: 清除定时器
   */
  private clearTimeOut() {
    clearTimeout(this.timeTicket)
    this.timeTicket = null
  }
}
</script>
<style lang="scss" scoped>
.gee-mark-digitalscroll {
  display: flex;
  justify-content: center;
  line-height: 100%;
  height: 100%;
  p {
    line-height: 100%;
    // flex: 1;
    width: 12px;
    height: 100%;
    text-align: center;
    display: inline-block;
    font-size: 20px;
    position: relative;
    writing-mode: vertical-lr;
    text-orientation: upright;
    overflow: hidden;
    &:last-child {
      margin-right: 0;
    }
    span {
      position: absolute;
      top: 0;
      left: 50%;
      transform: translate(-50%, 0%);
      transition: transform 2s ease 1s;
      font-weight: 500;
      letter-spacing: 10px;
      font-family: DigifaceWide;
    }
  }
}
</style>