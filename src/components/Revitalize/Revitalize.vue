<template>
  <div class="revitalize">
    <rheader v-if="false" />
    <transition name="fade">
      <mapbox />
    </transition>
    <rlayout v-if="false" />
    <smoke v-if="false" />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import RevitalizeHeader from "./components/RevitalizeHeader.vue";
import RevitalizeLayout from "./components/RevitalizeLayout.vue";
import Mapbox from "./Mapbox/Mapbox.vue";
import Smoke from "./components/Smoke.vue";

@Component({
  components: {
    ["rheader"]: RevitalizeHeader,
    ["rlayout"]: RevitalizeLayout,
    ["mapbox"]: Mapbox,
    ["smoke"]: Smoke
  }
})
export default class Revitalize extends Vue {
  private doLine = true;
  mounted() {
    this.eventRegister();
  }
  private eventRegister(): void {
    const { $hub } = this as any;
  }
}
</script>

<style scoped lang="less">
.revitalize {
  height: 100%;
  position: relative;
}
.revitalize:before {
  display: none;
  content: "";
  position: fixed;
  bottom: 0;
  height: 140px;
  width: 100%;
  left: 0;
  background: linear-gradient(
    to top,
    rgba(255, 255, 255, 0.8),
    rgba(0, 0, 0, 0)
  ) !important;
  z-index: 2;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
