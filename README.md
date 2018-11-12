# panel
一个可调节刻度(指标)并触发事件的刻度尺

效果图如下：
![Image text](https://github.com/xiaoming-2018/panel/blob/master/img-folder/panel.jpg)

使用方式如下：
![Image text](https://github.com/xiaoming-2018/panel/blob/master/img-folder/container.jpg)

使用方式代码：
```
<template>
  <div class="test" style="width: 1440px; text-align: center">
    <panel></panel>
  </div>
</template>

<script>
  import panel from './panel'
  export default {
    data () {
      return {}
    },
    components:{
      panel,
    },
  }
</script>

<style scoped>

</style>
```
