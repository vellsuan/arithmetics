<template>
  <div v-show="show" class="counter-warp" @click="increment"  >
    <div :class="{'counter-bg': true,'counter-bgs': list}" ref="counterbg">
      <div   :class="{content: true}" >
        <div class="title">
          <div>请填写你的小区、写字楼等</div>
          <div @click="hide" class="xbg"></div>
        </div>
        <div class="seach">
          <div class="word">
            <div class="seach_icon"></div>
            <input type="text"  v-model="areaval" v-on:input="area" placeholder="输入内容"></div>
        </div>
        <div  class="content_item">
          <div v-for="(item, index) in addressList" :key="index" class="district">
            <div>{{item.name}}</div>
            <div @click="selectArea(item)" class="detail">{{item.city+item.district}}</div>
          </div>
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
      increment () {
        this.list = true
      },
      hide () {
        this.list = false
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
        list: true,
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
    border-radius:0;
  }
  .counter-bg{
    background: white;
    height: 400px;
    width: 100%;
    position: fixed;
    bottom: -400px;
    transition:bottom 1s;
    z-index: 99999;
  }
  .counter-bgs{
    background: white;
    height: 400px;
    width: 100%;
    position: fixed;
    bottom: 0;
  }
  .content {
    background:white;
    transition:width 2s;
    -moz-transition:width 2s; /* Firefox 4 */
    -webkit-transition:width 2s; /* Safari and Chrome */
    -o-transition:width 2s; /* Opera */
  }
  .contents
  {
    height:100px;
    width:300px;
    background:white;
    transition:width 2s;
    -moz-transition:width 2s; /* Firefox 4 */
    -webkit-transition:width 2s; /* Safari and Chrome */
    -o-transition:width 2s; /* Opera */
  }
  .word{
    width: 100%;
    height: 100%;
    display: flex;
    line-height: 30px;
    text-indent: 34px;
    border-radius: 5px;
    border: 1px solid #E5E5E5;
    -webkit-appearance: none;
    align-items: center;
  }
  .content_item{
    height: 320px;
   overflow: auto;
  }
  .title{
    background: #f44242;
    height: 42px;
    display: flex;
    line-height: 42px;
    font-size: 12px;
    padding: 0 10px;
    justify-content: space-between;
    align-items: center;
  }
  .seach{
    text-align: left;
    background: #F7F7F7;
    padding: 7px 10px;
    border-top: 1px solid #e5e5e5;
    color: #333;
  }
  .district{
    text-align: left;
    font-size: 14px;
    padding:10px;
    background: #fff;
    border-bottom: 1px solid #e5e5e5;
    color: #333;
  }
  .detail{
    font-size: 13px;
    color: #666;
  }
  .xbg{
    height: 30px;
    width: 30px;
    background: url(../../static/images/address_icon.png) no-repeat center center;
    background-size: 50%;
    top: 6px;
  }
  .seach_icon{
    background: url(../../static/images/category-search.png) no-repeat center center;
    background-size: 100%;
    height: 16px;
    width: 16px;
    margin-left: 10px;
    margin-right: 10px;
  }
  input{
    width: 100%;
    text-align: left;
  }

 </style>
