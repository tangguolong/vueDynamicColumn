<template>
  <div id="app">
    <DynamicColumn :data="data"></DynamicColumn>
    <router-view />
  </div>
</template>

<script>
import DynamicColumn from './dynamicPlugin/dynamicColumn'
export default {
  name: 'App',
  components: {
    DynamicColumn
  },
  data() {
    return {
      data: {
        currentYear: 1998,
        yAxis: [500, 400, 300, 200, 100],
        xAxis: {
          1998: [
            {
              xAxisName: '白菜',
              value: 100
            },
            {
              xAxisName: '花椰菜',
              value: 80
            },
            {
              xAxisName: '椰子菜',
              value: 80
            }
          ],
          1999: [
            {
              xAxisName: '白菜',
              value: 120
            },
            {
              xAxisName: '花椰菜',
              value: 280
            },
            {
              xAxisName: '椰子菜',
              value: 180
            }
          ],
          2000: [
            {
              xAxisName: '白菜',
              value: 40
            },
            {
              xAxisName: '花椰菜',
              value: 30
            },
            {
              xAxisName: '椰子菜',
              value: 180
            }
          ],
          2001: [
            {
              xAxisName: '白菜',
              value: 400
            },
            {
              xAxisName: '花椰菜',
              value: 320
            },
            {
              xAxisName: '椰子菜',
              value: 180
            }
          ]
        }
      }
    }
  },
  mounted() {}
}
</script>