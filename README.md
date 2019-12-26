# vueDynamicColumn
vue动态播放柱形变化图插件

效果：
能够轮播柱形图数据且排序，最大数据在最左边

数据格式：
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
          ]
        }
      }

