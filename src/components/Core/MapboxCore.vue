<template>
  <div />
</template>
<script lang="ts">
/**
 * created by eds 2019.12.18
 * common mapbox root component
 */
import { Component, Prop, Vue } from "vue-property-decorator";
// import { Map, MapboxOptions } from "mapbox-gl/index";
import EchartLayer from "echartslayer";
import mapboxOptions from "@/config/mapbox_zj";
const _CENTER_ = [120.66052090621764, 26.010845855663625];
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
export default class MapboxCore extends Vue {
  @Prop({ default: "MAPBOX_MAP" }) private mapId!: number;
  //    esri object <T>
  public map: any;
  public chart: any;

  //    initMap Promise<Boolean>
  public initMap(mapId: string, fn?: Function): Promise<boolean> {
    return new Promise(resolve => {
      this.map = new (window as any).mapboxgl.Map({
        container: mapId,
        ..._MAPBOX_OPTION_,
        style: mapboxOptions
      } as any);
      this.map.on("style.load", () => {
        fn && fn();
        this.chart = new EchartLayer(this.map);
        this.chart.chart.setOption({
          title: {
            text: "全国主要城市空气质量",
            subtext: "data from PM25.in",
            sublink: "http://www.pm25.in",
            left: "center",
            textStyle: {
              color: "#fff"
            }
          },
          tooltip: {
            trigger: "item"
          },
          legend: {
            orient: "vertical",
            y: "bottom",
            x: "right",
            data: ["pm2.5","Top 5"],
            textStyle: {
              color: "#fff"
            }
          },
          GLMap: {},

          series: [
            {
              name: "pm2.5",
              type: "scatter",
              coordinateSystem: "GLMap",
              data: [
                {
                  name: "b",
                  value: [120.66052090621764, 28.010845855663625, 10]
                }
              ],
              symbolSize: 10,
              label: {
                normal: {
                  formatter: "{b}",
                  position: "right",
                  show: false
                },
                emphasis: {
                  show: true
                }
              },
              itemStyle: {
                normal: {
                  color: "#ddb926"
                }
              }
            },
            {
              name: "Top 5",
              type: "effectScatter",
              coordinateSystem: "GLMap",
              data: [
                {
                  name: "a",
                  value: [120.66052090621764, 28.010845855663625, 20]
                }
              ],
              symbolSize: 20,
              showEffectOn: "render",
              rippleEffect: {
                brushType: "stroke"
              },
              hoverAnimation: true,
              label: {
                normal: {
                  formatter: "{b}",
                  position: "right",
                  show: true
                }
              },
              itemStyle: {
                normal: {
                  color: "#f4e925",
                  shadowBlur: 10,
                  shadowColor: "#333"
                }
              },
              zlevel: 1
            }
          ]
        });
        console.log(this.chart);
        return resolve(true);
      });
    });
  }
}
</script>