<template>
  <div class="map">
    <GoogleMap
      api-key="AIzaSyCcfx4-W3siKrad3OszOuKrL8n48QK1oxE"
      style="width: 100%; height: 500px"
      ref="map"
      :center="center"
      :zoom="6"
    >
      <div class="a" v-for="(posity, i) in positions" :key="i">
        <Marker :options="{ position: posity }" />
      </div>
      <Polyline :options="{path: positions, strokeOpacity: 0.3}" v-if="positions[0].lat" />
    </GoogleMap>
  </div>
</template>
<script lang="ts">
import { Options, Vue } from "vue-class-component";
import { GoogleMap, Marker, Polyline } from "vue3-google-map";

@Options({
  components: {
    GoogleMap,
    Marker,
    Polyline
  },
  props: {
    points: {
      type: Array,
      required: true,
    },
  },
})
export default class HomeView extends Vue {
  get positions() {
    let map = this.$refs.map as any
    if(map) {
      let bounds = new map.api.LatLngBounds();
      this.points.forEach((point: any) => {
        bounds.extend(point);
      });
      map.map.fitBounds(bounds);
    }
    return this.points;
  }
  get center() {
    let lat = 50;
    let lng = 14;
    return { lat, lng };

    // this.positions.forEach((pos) => {
    //   lat += pos.lat;
    //   lng += pos.lng;
    // });
    
    // lat = lat / this.positions.length;
    // lng = lng / this.positions.length;
    
  }

  points!: any[];
}
</script>
