<template>
  <view class="content">
    12
    <view class="text-area">
          <canvas canvas-id="column" id="column" class="charts" @click="tap"
          />
    </view>
  </view>
</template>

<script setup lang="ts">

import uCharts from '@qiun/ucharts';
import { onMounted, ref } from 'vue'

type TTargetPlus = Pick<MouseEvent, 'target'> & { id: string } | null;

interface IData {
    categories: string[];
    series: {
        name: string;
        data: number[];
    }[];
}

const uChartsInstance: Record<string, IEchartInstance> = {};
const cWidth = 750;
const cHeight = 500;

const getServerData = () => {
      //模拟从服务器获取数据时的延时
      setTimeout(() => {
        //模拟服务器返回数据，如果数据格式和标准格式不同，需自行按下面的格式拼接
        const res: IData = {
            categories: ["2016","2017","2018","2019","2020","2021"],
            series: [
              {
                name: "目标值",
                data: [35,36,31,33,13,34]
              },
              {
                name: "完成量",
                data: [18,27,21,24,6,28]
              }
            ]
          };
        drawCharts('column', res);
      }, 500);
    }
    
    const drawCharts = (id: string, data: IData) => {
      const ctx = uni.createCanvasContext(id);
      uChartsInstance[id] = new uCharts({
        type: "column",
        context: ctx,
        width: cWidth,
        height: cHeight,
        categories: data.categories,
        series: data.series,
        animation: true,
        background: "#FFFFFF",
        padding: [15,15,0,5],
        xAxis: {
          disableGrid: true
        },
        yAxis: {
          data: [{min: 0}]
        },
        extra: {
          column: {
            type: "group"
          }
        }
      });
    }
    const tap = (e: MouseEvent) => {
      const target = e.target as TTargetPlus;
      if (target?.id) {
        uChartsInstance[target.id].touchLegend(e);
        uChartsInstance[target.id].showToolTip(e);
      }
    }

onMounted(() => {
    getServerData();
})
</script>

<style scoped>
  .charts{
    width: 750rpx;
    height: 500rpx;
  }
</style>