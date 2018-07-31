<template>
  <div class="page">
    <div :class="{'pickerMask':isShowMask}"></div>
    <div class="default" @click="showPickerView">
      <span class="paleft">{{columuOne[0]}}</span><span class="paleft">{{columnTwo[0]}}</span><span class="paleft">{{columnThree[three]}}</span>
    </div>
    <div class="weui-picker" :class="{'weui_picker_view_show':pickerShow}">
      <div class="weui-picker__hd">
        <div href="javascript:;" class="weui-picker__action" @click="pickerCancel">取消</div>
        <div href="javascript:;" class="weui-picker__action" @click="pickerConfirm">确定</div>
      </div>
      <picker-view indicator-style="height: 40px;" :value="pickerValue" class="weui_picker_view" @change="pickerChange">
        <picker-view-column>
          <div class="picker-item" v-for="item in columuOne" :key="index">{{item}}</div>
        </picker-view-column>
        <picker-view-column>
          <div class="picker-item" v-for="item in columnTwo" :key="index">{{item}}</div>
        </picker-view-column>
        <picker-view-column>
          <div class="picker-item" v-for="item in columnThree"  :key="index">{{item}}</div>
        </picker-view-column>
      </picker-view>
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      show: {type: Boolean, default: false}, // 是否出现
      listdata: {type: Array, default: []},
      regions: {type: String, default: '北京'},
      districtCode: {type: String, default: '001001001001'}
    },
    data () {
      return {
        pickerShow: false,
        isShowMask: false,
        pickerValue: [0, 1, 2],
        mulLinkAgeArray: [{
          'name': '北京',
          'id': '001001',
          'child': [
            {
              'name': '北京市',
              'id': '001001001',
              'child': [
                {
                  'name': '东城区',
                  'id': '001001001001'
                }, {
                  'name': '西城区',
                  'id': '001001001002'
                }, {
                  'name': '朝阳区',
                  'id': '001001001005'
                }, {
                  'name': '丰台区',
                  'id': '001001001006'
                }, {
                  'name': '石景山区',
                  'id': '001001001007'
                }, {
                  'name': '海淀区',
                  'id': '001001001008'
                }, {
                  'name': '门头沟区',
                  'id': '001001001009'
                }, {
                  'name': '房山区',
                  'id': '001001001010'
                }, {
                  'name': '通州区',
                  'id': '001001001011'
                }, {
                  'name': '顺义区',
                  'id': '001001001012'
                }, {
                  'name': '昌平区',
                  'id': '001001001013'
                }, {
                  'name': '大兴区',
                  'id': '001001001014'
                }, {
                  'name': '怀柔区',
                  'id': '001001001015'
                }, {
                  'name': '平谷区',
                  'id': '001001001016'
                }, {
                  'name': '密云县',
                  'id': '001001001017'
                }, {
                  'name': '延庆县',
                  'id': '001001001018'
                }
              ]
            }
          ]
        }
        ],
        columuOne: [],
        columnTwo: [],
        columnThree: [],
        one: '',
        two: '',
        three: 0
      }
    },
    mounted () {
      this._initPicker()
    },
    methods: {
      pickerChange: function (e) {
        let _this = this
        console.log(e)
        let value = e.mp.detail.value
        this.three = value[2]
        let code = this.mulLinkAgeArray[0].child[0].child[this.three].id
        this.$emit('val', code)
        // 如果是第一列滚动
        if (value[0] !== _this.pickerValue[0]) {
          let columnTwoNew = _this.mulLinkAgeArray[value[0]].child
          _this.columnTwo = []
          for (let i = 0; i < columnTwoNew.length; i++) {
            _this.columnTwo.push(columnTwoNew[i].name)
          }
          _this.pickerValue = value
          _this.pickerValue[1] = 0
        }
        // 如果第二列滚动
        if (value[1] !== this.pickerValue[1]) {
          _this.pickerValue[1] = e.mp.detail.value[1]
        }
        // 如果第三列滚动
        if (value[2] !== this.pickerValue[2]) {
          _this.pickerValue[2] = e.mp.detail.value[2]
        }
      },
      pickerConfirm () {
        this.isShowMask = false
        this.pickerShow = false
      },
      pickerCancel () {
        this.isShowMask = false
        this.pickerShow = false
      },
      showPickerView () {
        this.isShowMask = true
        this.pickerShow = true
      },
      _initPicker () {
        let _this = this
        let mulLinkAgeArray = this.mulLinkAgeArray
        for (let i = 0; i < mulLinkAgeArray.length; i++) {
          _this.columuOne.push(mulLinkAgeArray[i].name)
        }
        // 渲染第二列
        let columnTwoArray = mulLinkAgeArray[_this.pickerValue[0]].child
        for (let i = 0; i < columnTwoArray.length; i++) {
          _this.columnTwo.push(columnTwoArray[i].name)
        }
        // 渲染第三列
        let columnTherrArray = mulLinkAgeArray[_this.pickerValue[0]].child[0].child
        for (let i = 0; i < columnTherrArray.length; i++) {
          _this.columnThree.push(columnTherrArray[i].name)
        }
        let i = 0
        for (let item of mulLinkAgeArray[0].child[0].child) {
          i++
          if (item.id === this.districtCode) {
            this.three = i
            console.log(this.three)
          }
        }
      }
    }
  }
</script>

<style scoped>
  page {
    margin-top: 100px;
    position: relative;
  }
  .weui-picker {
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    transition: all 0.3s ease;
    transform: translateY(100%);
    z-index: 3000;
  }
  .weui_picker_view {
    position: relative;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 238px;
    background-color: rgba(255, 255, 255, 1);
  }
  .weui-picker__hd {
    display: flex;
    padding: 9px 15px;
    background-color: #fff;
    position: relative;
    text-align: center;
    font-size: 17px;
  }
  .weui-picker__action {
    display: block;
    flex: 1;
    color: #1aad19;
    cursor: pointer;
  }
  .weui-picker__action:first-child {
    text-align: left;
    color: #888;
  }
  .weui-picker__action:last-child {
    text-align: right;
  }
  .weui-picker__hd:after {
    content: " ";
    position: absolute;
    left: 0;
    bottom: 0;
    right: 0;
    height: 1px;
    border-bottom: 1px solid #e5e5e5;
    color: #e5e5e5;
    transform-origin: 0 100%;
    transform: scaleY(0.5);
  }
  .weui_picker_view_show {
    transform: translateY(0);
  }
  .picker-item {
    text-align: center;
    line-height: 40px;
  }
  .pickerMask {
    position: fixed;
    z-index: 1000;
    top: 0;
    right: 0;
    left: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.6);
  }
  .default{
    width: 100%;
    height: 100%;
    background: #ffffff;
  }
  .paleft{
    padding-left: 20px;
    color: #787878;
  }
</style>
