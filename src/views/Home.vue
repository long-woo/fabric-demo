<template>
  <div class="home">
    <div class="fabric-container">
      <canvas id="elCanvas"></canvas>
    </div>
    <ul class="tool-container">
      <li title="撤销">
        <i class="iconfont">&#xe60c;</i>
      </li>
      <li @click="onChooseColor"
          title="颜色">
        <i class="iconfont">&#xe822;</i>
        <ColorPicker class="color-pikcer-container"
                     v-model="color"
                     v-show="showColorPikcer" />
      </li>
      <li title="笔"
          @click="onPenDrawing">
        <i class="iconfont">&#xe63e;</i>
      </li>
      <li title="文字"
          @click="onText">
        <i class="iconfont">&#xe6e0;</i>
      </li>
      <li title="橡皮擦">
        <i class="iconfont">&#xe604;</i>
      </li>
    </ul>
  </div>
</template>

<script>
import { fabric } from 'fabric'
import { Sketch } from 'vue-color'

export default {
  name: 'Home',
  components: {
    ColorPicker: Sketch
  },
  data () {
    return {
      canvas: null,
      color: {
        hex: '#000'
      },
      showColorPikcer: false
    }
  },
  watch: {
    'color.hex' (val) {
      const canvas = this.canvas

      if (canvas) {
        canvas.freeDrawingBrush.color = val || '#000'
      }
    }
  },
  mounted () {
    this.initFabirc()
  },
  methods: {
    initFabirc () {
      const oldElCanvas = document.querySelector('#elCanvas')
      oldElCanvas.width = window.innerWidth
      oldElCanvas.height = document.querySelector('.fabric-container').clientHeight

      const canvas = new fabric.Canvas('elCanvas')

      canvas.on('mouse:up', e => {
        console.log(e)
      })

      fabric.Image.fromURL('https://test-oss.xiaoheiban.cn/202002/4ba44416-ff43-4be9-8199-3891d13992e9.png', img => {
        if (img.width >= canvas.width || img.height >= canvas.height) {
          img.scaleToHeight(img.height / 2, false) // 缩放图片高度
          img.scaleToWidth(img.width / 2, false) // 缩放图片宽度
        }
        canvas.add(img)
      })

      this.canvas = canvas
    },

    // 选择颜色
    onChooseColor () {
      this.showColorPikcer = !this.showColorPikcer
    },

    // 画笔
    onPenDrawing () {
      const canvas = this.canvas

      if (canvas) {
        canvas.isDrawingMode = true
        canvas.freeDrawingBrush.width = 3
        // canvas.setCursor()
      }
    },

    // 添加文字
    onText (event) {
      console.log(event)
      // const textbox = new fabric.Textbox('')
    }
  }
}
</script>

<style lang="scss" scoped>
.home {
  flex: 1;
  display: flex;
  flex-direction: column;

  .tool-container {
    list-style: none;
    padding: 10px;

    li {
      display: inline-block;
      width: 30px;
      height: 30px;
      cursor: default;

      &:hover {
        color: #42b983;
      }

      .iconfont {
        font-size: 25px;
      }
    }
  }

  .fabric-container {
    flex: 1;
  }

  .color-pikcer-container {
    position: absolute;
    bottom: 50px;
  }
}
</style>
