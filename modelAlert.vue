<template>
  <div v-show="show" class="counter-warp" @click="increment"  >
    <div :class="{'counter-bg': true}" >
      <div class="alert_Bg">
        <div class="title">
          <div @click="hide" class="xbg"></div>
        </div>
        <div  class="content_item">
          <img src="../../static/images/close.png" alt="">
        </div>
        <div class="describe">使用规则：价值1-------------------------------</div>
        <div>
          <button class="btn" @click="close">关闭</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      show: {type: Boolean, default: false}, // 是否出现
      listdata: {type: Array, default: []},
      regions: {type: String, default: '北京'}
    },
    data () {
      return {
        list: true,
        newCount: 0,
        // isHave: true,
        showReduce: true,
        newProduct: {},
        areaval: '',
        addressList: []
      }
    },
    computed: {
    },
    mounted () {},
    methods: {
      hide () {
        this.$emit('hide', false)
      },
      close () {
        this.$emit('hide', false)
      },
      area () {
        const param = {
          url: '/map/getUserAddress',
          data: {
            region: this.regions,
            q: this.areaval
          },
          callback: d => {
            this.addressList = d.result
          }
        }
        this.$sendRequest(param)
      },
      selectArea (item) {
        this.$emit('area', item)
      }
    },
    destroyed () {

    },
    onShow () {
      Object.assign(this.$data, {
        list: false,
        newCount: 0,
        // isHave: true,
        showReduce: true,
        newProduct: {},
        areaval: '',
        addressList: []
      })
    }
  }
</script>

<style scoped>
  .counter-warp{
    width: 100%;
    background-color:rgba(0,0,0,0.8);
    height: 100%;
    bottom: 0;
    left: 0;
    position: fixed;
    z-index: 998;
    border-radius: 0px;
  }
  .counter-bg{
    height: 300px;
    width: 86%;
    z-index: 99999;
    position:absolute;
    right: 0;
    bottom: 20%;
    margin:auto;
    border-radius: 5px;
    left: 0;
    top: 0;
    background-color: #fff;
  }
  .content_item{
    color: black;
  }
  .title{
    background: #fff;
    height: 42px;
    display: flex;
    line-height: 42px;
    font-size: 12px;
    padding: 0 10px;
    justify-content: flex-end;
    align-items: center;
    border-radius: 5px;
  }
  .xbg{
    height: 30px;
    width: 30px;
    background: url(../../static/images/close.png) no-repeat center center;
    background-size: 50%;
    top: 6px;
  }

  .describe{
    color: black;
  }
  .btn{
    width: 80%;
    height: 30px;
    line-height: 30px;
    color: #fff;
    margin-top: 20px;
    background-color: #b1292d;
    bottom: 20px;
  }
  .alert_Bg{
    background: #fff;
    border-radius: 5px;
  }
 </style>
