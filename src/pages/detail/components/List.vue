<template>
  <div>
    <div class="item">
      <div class="item-title">{{list.brief}}</div>
      <div class="item-location">{{list.location}}</div>
      <div class="row-wrapper">
        <span class="grey">基本工资：</span>
        <span class="black">{{list.reward}}元/{{list.rewardType}}</span>
      </div>
      <div class="color-block"></div>
      <div class="row-wrapper">
        <span class="grey">兼职类型：</span>
        <span class="black">{{list.jobType}}</span>
      </div>
      <div class="row-wrapper">
        <span class="grey">招聘人数：</span>
        <span class="black">{{list.hireNum}}</span>
      </div>
      <div class="row-wrapper">
        <span class="grey">已聘人数：</span>
        <span class="black">{{list.nowNum}}</span>
      </div>
      <div class="color-block"></div>
      <div class="detail-title">工作内容：</div>
      <div class="detail">{{list.details}}</div>
      <div class="tip">若发现实际工作内容与上述描述不符，请您提高警惕并第一时间向我们举报</div>
      <div class="color-block"></div>
      <div class="row-wrapper">
        <span class="grey">联系人：</span>
        <span class="black">{{list.merchantName}}</span>
      </div>
      <div class="row-wrapper">
        <span class="grey">联系方式：</span>
        <span class="black">{{list.merchantPhone}}</span>
      </div>
      <div class="warn border-bottom">(提示：凡收取费用的兼职，需谨慎 ! )</div>
    </div>
  </div>
</template>

<script>
import bus from '@/assets/eventBus.js'
import axios from 'axios'
import qs from 'qs'
export default {
  name: 'DetailList',
  data () {
    return {
      list: {
        brief: '',
        location: '',
        reward: '',
        rewardType: '',
        jobType: '',
        hireNum: '',
        nowNum: '',
        details: '',
        merchantName: '',
        merchantPhone: '',
        merchantId: '',
        isSignOrNot: ''
      }
    }
  },
  methods: {
    sendRequest () {
      axios.post('http://yian.our16.top:8080/yian/parttimeHall/getParttimeDetail.do', qs.stringify({
        jobId: this.$route.params.jobId
      }), {
        withCredentials: true
      })
        .then(this.sendRequestSucc)
        .then(() => {
          bus.$emit('emitParams', this.list.merchantId, this.list.jobId, this.list.isSignOrNot)
        })
    },
    sendRequestSucc (res) {
      this.list = res.data.data
    }
  },
  mounted () {
    this.sendRequest()
  },
  created () {
    bus.$on('addNum', (n) => {
      this.list.nowNum += n
    })
    bus.$on('delNum', (n) => {
      this.list.nowNum -= n
    })
  },
  beforeDestroy () {
    bus.$off()
  }
}
</script>

<style lang="stylus" scoped>
@import '~@/assets/styles/varibles.styl'
  .item-title
    font-size .38rem
    padding .4rem .4rem
  .item-location
    font-size .24rem
    color grey
    padding-left .4rem
    padding-bottom .1rem
    border-bottom 1px solid #E9E9E9
  .row-wrapper
    padding .2rem .4rem
    .grey
      font-size .28rem
      color #979797
    .black
      font-size .32rem
  .color-block
    height .25rem
    background-color #EFEFF4
  .detail-title
    margin .3rem .4rem .2rem .4rem
    border-left .1rem solid $bgColor
    padding-left .2rem
    font-size .32rem
    color #0F87FF
  .detail
    padding .1rem .4rem
    font-size .28rem
    line-height .5rem
  .tip
    padding 0 .4rem .3rem .4rem
    font-size .24rem
    font-weight bold
  .warn
    padding .1rem .4rem .3rem .4rem
    color #FF4040
    margin-bottom 1rem
</style>
