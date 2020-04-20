<template>
  <div class="mapbox_map" :id="MapId" />
</template>

<script lang="ts">
import { Component, Mixins, Watch } from "vue-property-decorator";
import MapboxCore from "@/components/Core/MapboxCore.vue";
import { State } from "vuex-class";

@Component({
  methods: {}
})
export default class MapboxMap extends Mixins(MapboxCore) {
  protected id: number = +new Date();
  @State("arcgisDone") stateArcgisDone!: boolean;
  @Watch("stateArcgisDone")
  async arcgisDoneChange(value: boolean) {
    if (value) {
      await this.initMap(this.MapId);
    }
  }
  //    Getter/
  get MapId(): string {
    return "mapbox_" + this.id;
  }
  //  mounted
  async mounted(): Promise<void> {
    if (this.stateArcgisDone) {
      await this.initMap(this.MapId);
    }
    this.eventRegister();
  }
  private eventRegister(): void {
    const { $hub } = this as any;
  }
}
</script>

<style lang="less">
.mapbox_map {
  width: 100%;
  height: 100%;
  position: relative;
  .mapboxgl-canvas {
    left: 0;
    top: 0;
  }
}
</style>
