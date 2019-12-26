<template>
  <div class="wp">
    <div>
      <button @click="play()"
        v-if="playState">播放</button>
      <button @click="stop()"
        v-else>暂停</button>
    </div>
    <div class="container"
      :style="{width:showData.length<4?240+'px':showData.length*60+'px'}">
      <div class="topLeft"
        :style="{height:data.yAxis.length*50+'px!important'}">
        <ul>
          <li v-for="(item,index) in data.yAxis"
            :key="index"
            :style="{height:50+'px',borderTop:'1px solid #ccc'}">
            <span :style="{lineHeight:50+'px'}">{{item}}</span>
            <!-- <div style="height:10px;width:10px;border-top:1px solid #ccc;"></div> -->
          </li>
        </ul>
      </div>
      <div class="topright"
        :style="{height:(data.yAxis.length)*50+'px!important'}">
        <span class="yearTitle"
          :style="{left:showData.length<4?240+'px':showData.length*60+'px'}">{{currentYear}}</span>
        <ul class="x-axis">
          <li v-for="(item,index) in showData"
            :key="item.xAxisName"
            :style="{left: item.split + 'px',transitionProperty:'left',transitionDuration:'1s'}"
            :position="item.position">
            <p class="number"
              :style="{bottom: item.value/2 + 'px'}">
              {{item.value}}
            </p>
            <p class="bar"
              :style="{height: item.value/2 + 'px'}"></p>
            <p class="name">{{item.xAxisName | wordBreak}}</p>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
// import countTo from 'vue-count-to'
export default {
  name: 'dynamicColumn',
  props: {
    data: {
      type: Object
    }
  },
  data() {
    return {
      currentYear: this.data.currentYear,
      timer: '',
      showData: [],
      playState: true
    }
  },
  watch: {},
  methods: {
    play() {
      this.playState = false
      this.timer = setInterval(() => {
        //一个是设置当前年份，判断是否存在当前数组，如果让大于就重头，小于就持续递增
        let arrlen = Object.keys(this.data.xAxis)
        let arr = arrlen.filter(item => {
          return item > this.currentYear
        })
        if (arr.length > 0) {
          this.currentYear = Number(this.currentYear) + 1
        } else {
          this.currentYear = arrlen[0] //回头
        }
        // this.showData = this.data.xAxis[this.currentYear]
        this.rangeSort()
        //一个是过滤数据
      }, 1500)
    },
    //暂停
    stop() {
      this.playState = true
      clearInterval(this.timer)
    },
    rangeSort() {
      let data = this.data.xAxis[this.currentYear]
      let newData = JSON.parse(JSON.stringify(data))
      let arr = []
      let newarr = []
      let changeArr = newData
      data.map(item => {
        arr.push(Number(item.value))
      })
      arr.sort(function(a, b) {
        return -(a - b)
      })
      let rindex = []
      for (let i = 0; i < arr.length; i++) {
        newData = changeArr
        newData.map((item, index) => {
          if (arr[i] == item.value) {
            newarr.push(item)
            rindex.push(1)
            this.$set(item, 'split', rindex.length * 37)
            this.$set(item, 'position', i + 1)
            changeArr.splice(index, 1)
          }
        })
      }
      this.showData = newarr
    }
  },
  filters: {
    wordBreak(val) {
      return val.toString().length > 3 ? val.substring(0, 3) + '...' : val
    }
  },
  mounted() {
    this.rangeSort()
  }
}
</script>
<style lang="less" scoped>
.yearTitle {
  position: absolute;
  z-index: -1;
  font-size: 50px;
  color: #999;
}
button {
  margin: 0px 0px 10px 0px;
  width: 50px;
  height: 20px;
  color: #fff;
  border: 0px;
  background: #ff5172;
  background-color: red; /* 浏览器不支持时显示 */
  background-image: linear-gradient(#ffa467, #ff5372);
}
.container {
  display: flex;
}
.topLeft {
  width: 30px;
  height: 200px;
  color: #999;
  ul {
    margin: 0px;
    padding: 0px;
    position: absolute;
  }
  li {
    list-style: none;
  }
}
.topright {
  width: 82%;
  height: 200px;
  li {
    list-style: none;
  }
  border-bottom: 1px solid #ccc;
  border-left: 1px solid #ccc;
}
.bottomLeft {
  width: 10%;
  height: 100px;
}
.bottomright {
  width: 100px;
  height: 100px;
}
.x-axis {
  width: 100%;
  height: 100%;
  position: relative;
  li {
    position: absolute;
    z-index: 1;
    width: 15px;
    height: 100%;
    color: #999;
    font-size: 12px;
    text-align: center;
    transition: all 1s;
  }
  .number {
    position: absolute;
    width: 30px;
    left: -7.5px;
    transition: all 1s;
  }
  .bar {
    position: absolute;
    bottom: 0;
    width: 15px;
    height: 0;
    background: #ff5172;
    background-color: red; /* 浏览器不支持时显示 */
    background-image: linear-gradient(#ffa467, #ff5372);
    border-radius: 6px 6px 2px 2px;
    transition: all 1s;
  }
  .name {
    position: absolute;
    bottom: -65px;
    min-height: 50px;
    word-wrap: break-word;
    // letter-spacing: 20px;
    width: 10px;
  }
}
</style>
