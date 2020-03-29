<template>
  <div>
    <div class="configure">
    <h2>配置 configuration</h2>
    <p><input type="checkbox"
              v-model="enablePath"
              @change="renewGesture"
              checked><label >开启鼠标轨迹</label><span class="option-tip">enablePath</span></p>
    <p><span class="config-title">触发手势识别时间(ms)</span><input v-model="timeDelay"
                                                            type="number"
                                                            @change="renewGesture"><span class="option-tip">timeDelay</span></p>
    <p><span class="config-title">鼠标轨迹颜色</span><input v-model="lineColor"
                                                      type="text"
                                                      @change="renewGesture"><span class="option-tip">lineColor</span></p>
    <p><span class="config-title">鼠标轨迹宽度</span><input v-model="lineWidth" type="text"  @change="renewGesture"><span
      class="option-tip">lineWidth</span></p>
    <p><span class="config-title">触发手势识别鼠标按键</span><input v-model="triggerMouseKey"
                                                          type="text"
                                                          @change="renewGesture"><span class="option-tip">triggerMouseKey</span></p>
    <p><span class="config-title">开启手势时背景色</span><input v-model="activeColor"
                                                        type="text"
                                                        @change="renewGesture"><span class="option-tip">triggerMouseKey</span></p>

  </div>
    <div class="recognize">
    <div class="add">
      <h2>手势区域</h2>
      <label>添加新手势 </label>
      <input v-model="gestureName" placeholder="手势名称">
      <button  @click="addGesture">ADD</button>
    </div>
    <div class="result">
      <h2>识别结果</h2>
      <p>Gesture result: <span style="font-weight: bold; color: #2b5;">{{this.result}}</span></p>
      <p>Swipe directions result: <span style="font-weight: bold; color: #2b5;" >{{this.result0}}</span></p>
    </div>
    <div id="test" class="stage">
    </div>
  </div>
  </div>
</template>

<script>
import smartGesture from 'smart-gesture'
export default {
  name: 'HelloWorld',
  components: {
  },
  data () {
    return {
      canvas: {},
      result: '未识别',
      result0: '未识别',
      gestureName: '',
      lastPoints: [],
      enablePath: 'checked',
      timeDelay: 0,
      lineColor: '#666',
      lineWidth: 4,
      triggerMouseKey: 'left',
      activeColor: 'rgba(0, 0, 0, .05)',
    }
  },
  methods: {
    renewGesture () {
      const options = {
        enablePath: this.enablePath,
        timeDelay: this.timeDelay,
        lineColor: this.lineColor,
        lineWidth: this.lineWidth,
        triggerMouseKey: this.triggerMouseKey,
        activeColor: this.activeColor
      }
      // const canvas = new smartGesture(options)
      this.canvas.refresh(options)
      console.log('refresh success')
    },
    addGesture () {
      this.canvas.addGesture({
        name: this.gestureName,
        points: this.lastPoints
      })
      console.log('add success')
    }
  },
  mounted () {
    const options = {
      el: document.getElementById('test'),
      enablePath: this.enablePath,
      timeDelay: this.timeDelay,
      lineColor: this.lineColor,
      lineWidth: this.lineWidth,
      triggerMouseKey: this.triggerMouseKey,
      activeColor: this.activeColor,
      onSwipe: (list) => {
        this.result0 = list.join('')
        console.log(list)
      },
      onGesture: (res, points) => {
        console.log(res)
        this.result= res.score > 2 ? res.name : '未识别';
        this.lastPoints = points
      }
    }
    this.canvas = new smartGesture(options)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .configure {
    position: relative;
  }

  .configure,
  .recognize {
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 0 15px;
    margin-top: 10px;
  }

  .add,
  .result {
    display: inline-block;
    vertical-align: top;
  }

  .result {
    margin-left: 100px;
    padding-left: 100px;
    border-left: 1px dashed #ccc;
  }

  .config-title {
    display: inline-block;
    width: 200px;
  }

  .option-tip {
    margin-left: 10px;
    font-size: 12px;
    vertical-align: middle;
  }

  .stage {
    height: 400px;
    background: #ddd;
    position: relative;
    margin: 15px 0;
  }

  .stage:before {
    content: '绘制区域';
    color: #eeeeee;
    font-size: 30px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }

  img {
    width: 100%;
  }

  .build-in {
    position: absolute;
    width: 400px;
    right: 250px;
    top: 50%;
    transform: translateY(-50%);
  }

  .build-in:before {
    content: '内置图形';
    position: absolute;
    margin-top: -1.4em;
  }
</style>
