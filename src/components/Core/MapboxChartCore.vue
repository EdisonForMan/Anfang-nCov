<template>
  <div />
</template>
<script lang="ts">
/**
 * created by eds 2019.12.18
 * common mapbox root component
 */
import { Component, Prop, Vue } from "vue-property-decorator";
import echarts from "echarts";
import "echarts-gl";
// import { Map, MapboxOptions } from "mapbox-gl/index";
import mapboxOptions from "@/config/mapbox_zj";
const _CENTER_ = [120.66052090621764, 28.010845855663625];
const _ZOOM_ = 17;
const _MINZOOM_ = 9;
const _MAXZOOM_ = 18;
const _MAPBOX_OPTION_ = {
  center: _CENTER_,
  zoom: _ZOOM_,
  minZoom: _MINZOOM_,
  maxZoom: _MAXZOOM_
};

@Component({})
export default class MapboxChartCore extends Vue {
  @Prop({ default: "MAPBOX_MAP" }) private mapId!: number;
  //    esri object <T>
  public map: any;
  public chart: any;

  //    initMap Promise<Boolean>
  public initMap(mapId: string, fn?: Function): Promise<boolean> {
    return new Promise(resolve => {
      this.chart = echarts.init(
        document.getElementById(mapId) as HTMLDivElement | HTMLCanvasElement
      );
      this.chart.setOption({
        mapbox3D: {
          ..._MAPBOX_OPTION_,
          // style: "mapbox://styles/mapbox/dark-v9"
          style: mapboxOptions
        }
      });
      setTimeout(() => {
        this.chart.setOption({
          tooltip: {
            trigger: "item"
          },
          legend: {
            orient: "vertical",
            y: "top",
            x: "right",
            data: ["bar3D", "流向图"],
            textStyle: {
              color: "#fff"
            }
          },
          series: [
            {
              name: "bar3D",
              type: "bar3D",
              coordinateSystem: "mapbox",
              barSize: 1, //柱子粗细
              shading: "lambert",
              opacity: 1,
              bevelSize: 0.3,
              label: {
                show: true,
                formatter: "{b}",
                position:'top'
              },
              data: [{ name: "严重", value: [120.66052090621764, 28.011245855663625].concat(100) }]
            },
            {
              name: "流向图",
              type: "lines3D",
              coordinateSystem: "mapbox3D",
              effect: {
                show: true,
                period: 3, //箭头指向速度，值越小速度越快
                trailLength: 0.2, //特效尾迹长度[0,1]值越大，尾迹越长重
                symbolSize: 6 //图标大小
              },
              lineStyle: {
                normal: {
                  opacity: 1,
                  width: 2,
                  curveness: 0.4, //曲线的弯曲程度
                  color: "#609fd4"
                }
              },
              // blendMode: "lighter",

              data: [
                [_CENTER_, [120.5820262, 28.0787275]],
                [_CENTER_, [120.5894472, 28.02007991]],
                [_CENTER_, [120.7716305, 28.00273406]],
                [_CENTER_, [120.7507711, 27.96865542]],
                [_CENTER_, [120.610766853, 28.074736483]]
              ]
            }
          ]
        });
      }, 1000);
      this.map = this.chart
        .getModel()
        .getComponent("mapbox3D")
        .getMapbox();
      this.map.setCenter(_CENTER_);
      return resolve(true);
    });
  }
}
</script>