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
export default class MapboxCore extends Vue {
  @Prop({ default: "MAPBOX_MAP" }) private mapId!: number;
  //    esri object <T>
  public map: any;

  //    initMap Promise<Boolean>
  public initMap(mapId: string, fn?: Function): Promise<boolean> {
    return new Promise(resolve => {
      this.map = new (window as any).mapboxgl.Map({
        container: mapId,
        ..._MAPBOX_OPTION_,
        style: mapboxOptions,
        // style: {
        //   version: 8,
        //   name: "map",
        //   glyphs: "mapbox://fonts/mapbox/{fontstack}/{range}.pbf",
        //   //   "85b88ce10c15f390ee75bf571688b3b7",
        //   sources: {
        //     "raster-tiles": {
        //       type: "raster",
        //       tiles: [
        //         "http://t4.tianditu.gov.cn/cva_c/wmts?layer=cva&style=default&tilematrixset=c&Service=WMTS&Request=GetTile&Version=1.0.0&Format=tiles&TileMatrix={z}&TileCol={x}&TileRow={y}&tk=85b88ce10c15f390ee75bf571688b3b7",
        //         "http://t5.tianditu.gov.cn/cva_c/wmts?layer=cva&style=default&tilematrixset=c&Service=WMTS&Request=GetTile&Version=1.0.0&Format=tiles&TileMatrix={z}&TileCol={x}&TileRow={y}&tk=85b88ce10c15f390ee75bf571688b3b7",
        //         "http://t6.tianditu.gov.cn/cva_c/wmts?layer=cva&style=default&tilematrixset=c&Service=WMTS&Request=GetTile&Version=1.0.0&Format=tiles&TileMatrix={z}&TileCol={x}&TileRow={y}&tk={key}"
        //       ],
        //       tileSize: 256
        //     }
        //   },
        //   layers: [
        //     {
        //       id: "map",
        //       type: "raster",
        //       source: "raster-tiles"
        //     }
        //   ]
        // }
      });
      this.map.on("style.load", () => {
        fn && fn();
        return resolve(true);
      });
    });
  }
}
</script>