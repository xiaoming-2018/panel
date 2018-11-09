<template>
  <div>
    <div class="slider" ref="slider">
      <span id="plus" @click="plus">+</span>
      <div class="thunk" ref="trunk" :style="{bottom}">
        <div class="block"></div>
          <div class="tips">
            {{scale}}
            <i class="fas fa-caret-down" ></i>
          </div>
      </div>
      <span id="minus" @click="minus">-</span>
    </div>
    <div id="scale">
      <li v-for="item in items" class="item">
        <span>{{ item }}</span>
      </li>
    </div>
  </div>

</template>

<script>
export default {
  data () {
    return {
      slider:null,
      thunk:null,
      scale: 0.2, //初始化游标为0.2
      items: ['1','0.9','0.8','0.7','0.6','0.5','0.4','0.3','0.2','0.1','0']
    }
  },
  mounted () {
    let self = this;
    self.slider = self.$refs.slider;
    self.thunk = self.$refs.trunk;

    // 鼠标点下去的时候触发
    self.thunk.onmousedown = function (e) {
        let height = parseInt(self.height);   //当前点击的位置具体当前组件底部的高度
        let disY = e.clientY; //当前点击的y轴位置，注意：左边的原点在浏览器的左上角，所以越往下，y的值越大
        // 鼠标移动的时候触发
        document.onmousemove = function(e){
          let newHeight = disY-e.clientY + height; // 通过计算获得当前位置刻度轴底部的高度
          if(newHeight >= 0){ //鼠标位置在0刻度以上才去计算，否则不去处理
            let percent = parseFloat((newHeight / self.slider.offsetHeight).toFixed(1)) //计算当前刻度在整个刻度轴上的比例，
                                                                                        // 四舍五入保留一位小数
            if(percent <= 1.0 && percent >= 0.0){
              self.scale = percent  //把计算出来的刻度值赋给记录刻度的变量
            }
          }
        }
        //松开鼠标的时候触发
        document.onmouseup = function(){
          document.onmousemove = document.onmouseup = null; // 鼠标再移动也不去继续触发move操作
          self.getData(self.scale)  //根据调整后的刻度处理自己的业务
        }
        return false;
    }
  },
  computed:{
    height(){
      // 根据刻度动态计算当前当前刻度的高度
      let self = this
      if(self.slider){
        return self.slider.offsetHeight * self.scale + 8 + 'px';
      }else{
        return 0 + 'px'
      }
    },
   bottom(){
      // 动态计算距离底部的高度,减去的self.thunk.offsetHeight/2是中间刻度球的半径
      let self = this
      if(self.slider){
        return self.slider.offsetHeight * self.scale -  self.thunk.offsetHeight/2  + 'px';
      }else{
        return 0 + 'px'
      }
    }
  },
  watch:{
  },
  methods:{
    //  点击加号
    plus: function () {
      let self = this
      if(self.scale < 1){// 指标小于1的时候才继续操作
        self.scale = parseFloat((self.scale + 0.1).toFixed(1))//当前刻度值加0.1
        self.getData(self.scale)//根据调整后的刻度处理自己的业务
      }
    },
    //  点击减号
    minus: function () {
      let self = this
      if(self.scale > 0){// 指标大于0的时候才继续操作
        self.scale = parseFloat((self.scale - 0.1).toFixed(1))//当前刻度值减0.1
        self.getData(self.scale)//根据调整后的刻度处理自己的业务
      }
    },
    getData: function (target) {
      console.log('指标调整为: '+target)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.slider{
  position:fixed;
  margin-left:90px;
  width:10px;
  height:300px;
  background:#e4e7ed;
  border-radius:5px;
}
.slider .thunk{
  position:absolute;
  left:-4px;
  bottom:-7px;
  width:20px;
  height:20px;
  cursor: pointer;
}
.slider .block{
  width:16px;
  height:16px;
  border-radius:50%;
  border:1px solid #e4e7ed;
  background:rgba(255,255,255,1);
  transition:.2s all
}
.slider .tips{
  position:absolute;
  left:-50px;
  bottom:0px;
  min-width:15px;
  text-align:center;
  padding:4px 8px;
  background:#000;
  border-radius:5px;
  height:14px;
  line-height:14px;
  font-size: 12px;
  color:#fff}
.slider .tips i{
  position:absolute;
  margin-left:-5px;
  left:50%;
  bottom:-9px;
  font-size:16px;
  color:#000}
#scale{
  position: fixed;
  margin-left: 110px;
  height: 300px;
  margin-top: -8px;
}
.item{
  list-style: none;
  height: 30px;
  font-size: 13px;
}
#plus{
  position: absolute;
  top: -32px;
  left: -5px;
  font-size: 20px;
  width: 20px;
  height: 20px;
  line-height: 20px;
  border-radius: 50%;
  background-color: #e4e7ed;
  color: white;
  cursor: pointer;
  -webkit-user-select:none;
  -moz-user-select:none;
  -ms-user-select:none;
  user-select:none;
}
#minus{
  position: absolute;
  top: 310px;
  left: -5px;
  font-size: 20px;
  width: 20px;
  height: 20px;
  line-height: 20px;
  border-radius: 50%;
  cursor: pointer;
  background-color: #e4e7ed;
  color: white;
  -webkit-user-select:none;
  -moz-user-select:none;
  -ms-user-select:none;
  user-select:none;
}
</style>
