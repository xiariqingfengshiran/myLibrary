<template>
  <div id="box">
    <input type="text" placeholder="请输入搜索关键字" v-model="keyword" @keyup="get($event)" @keydown.down="changeDown()"
           @keydown.up.prevent="changeUp()"><span class="searchBtn" @click="get2()">搜索</span>
    <ul v-show="myData.length != 0">
        <li v-for="(value,index) in myData" :class="{bg:index==now}">
            {{value}}
        </li>
    </ul>
    <p v-show="myData.length==0">暂无数据...</p>
</div>
</template>


<script>
  import Vue from 'vue';
  import vueResource from 'vue-resource';
  Vue.use(vueResource);

export default {
  name:'HelloWorld',
  components:{

  },
  data: function (){
    return {
      myData: [],
      keyword: '',
      now: -1,
    }
  },
  methods: {
    get: function (event) {
      //按上下键时不发送请求
      if (event.keyCode == 38 || event.keyCode == 40)return;
      //按下回车键时发送请求
      if (event.keyCode == 13) {
        window.open('https://www.baidu.com/s?wd=' + this.keyword);
        //初始化搜索栏
        this.keyword = '';
      }

      this.$http.jsonp("https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su", {
        //传入的参数
        params: {
          wd: this.keyword,
        },
        jsonp: 'cb',

      }).then(function (res) {
        this.myData = res.data.s;
        //将json数据转成对象
      this.myData = JSON.parse(res.bodyText).s;
      }, function () {
        console.log("失败");
      });
    },
    get2:function(){
      window.open('https://www.baidu.com/s?wd=' + this.keyword);
      //初始化搜索栏
      this.keyword = '';
    },
    //向下按键
    changeDown: function () {
      var _this = this;
      _this.now++;
      if (_this.now == _this.myData.length) {
        _this.now = -1;
      }
      _this.keyword = _this.myData[_this.now];
    },
    //向上按键
    changeUp: function () {
      var _this = this;
      _this.now--;
      if (_this.now == -1) {
        _this.now = _this.myData.length;
      }
      _this.keyword = _this.myData[_this.now];
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
* {
            margin: 0;
            padding: 0;
            list-style: none;
            font-size: 16px;
        }

        #box {
            position:absolute;
            top:50%;
            left:50%;
            transform: translateX(-50%) translateY(-50%);
            width: 700px;
            height: 150px;
            /*border: 1px solid #999;*/
            box-sizing: border-box;
            text-align: center;
        }

        #box input {
            margin: 10px;
            height: 38px;
            box-sizing: border-box;
            outline: none;
        }

        #box > .searchBtn{
            display: inline-block;
            width: 92px;
            height: 38px;
            line-height: 38px;
            position: relative;
            top: 0;
            left: -11px;
            color:#fff;
            background-color:#19B955;
            cursor: pointer;
        }

        ul {
            position: relative;
            top: -10px;
            left: 104px;
            width: 400px;
            background-color: #fff;
            border: 1px solid #999;
            box-sizing: border-box;
            text-align: left;
        }

        input, li {
            width: 400px;
            height: 30px;
            line-height: 30px;
            text-indent:3px;
            box-sizing: border-box;
        }

        li{
          width:398px;
        }

        .bg {
            background-color: #EFEFEF;
        }
</style>
