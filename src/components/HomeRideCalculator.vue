<template>
  <div @keydown.enter="calculateThis()">
    <n-card title="Get a Ride!">
      Černošice
      <n-switch v-model:value="fromHome" size="large">
        <template #checked-icon>
          <ArrowForwardOutline></ArrowForwardOutline>
        </template>
        <template #unchecked-icon>
          <ArrowBackOutline></ArrowBackOutline>
        </template>
      </n-switch>
      Praha

      <NInput
        v-if="!fromHome"
        type="text"
        name="Origin"
        placeholder="Místo odjezdu"
        v-model="destination"
      />

      <n-popselect v-model:value="origin" :options="selection" class="abx">
        <n-button class="abx">{{ selection.filter(a => a.value == origin)[0].label || "Choose" }}</n-button>
      </n-popselect>

      <NInput
        v-if="fromHome"
        type="text"
        name="Origin"
        placeholder="Místo doručení"
        v-model="destination"
      />

      <div>
        <ul>
          <li>
            Distance <b>{{ distance }}</b>
          </li>
          <li>
            Liters <b>{{ liters }}</b>
          </li>
          <li>
            Wanted liter
            <b> {{ wanted }} </b>
          </li>
          <li>
            Price <b>{{ price }}</b>
          </li>
          <li>
            Cost <b>{{ cost }}</b>
          </li>
        </ul>
      </div>
    </n-card>

    <MapComponents :points="[pointOne, pointTwo]"></MapComponents>
  </div>
</template>

<script lang="ts">
import { Vue, Options } from "vue-class-component";
import axios from "axios";
import MapComponents from "./MapComponents.vue";
import { NInput, NCard, NSwitch, NPopselect, NButton } from "naive-ui";
import { ArrowBackOutline, ArrowForwardOutline } from "@vicons/ionicons5";

@Options({
  components: {
    MapComponents,
    NInput,
    NCard,
    NSwitch,
    ArrowBackOutline,
    ArrowForwardOutline,
    NPopselect,
    NButton,
  },
})
export default class Calculation extends Vue {
  destination = "";
  origin = "Brusinkova 1974, Cernosice";

  fromHome = true;

  distance = "0 km";
  liters = 0;
  wanted = 0;
  price = 0;
  cost = 0;
  enquiryId = 0;
  selection = [
    {label: "Brusinková 1974", value: "Brusinkova 1974, Cernosice"},
    {label: "Mokropeská", value: "Mokropeska 1713, Cernosice"}
  ]
  

  pointOne: LatLng = { lat: undefined, lng: undefined };
  pointTwo: LatLng = { lat: undefined, lng: undefined };

  async calculateThis() {
    const res = await axios.post("http://localhost:3000/enquiry", {
      origin: this.fromHome ? this.origin : this.destination,
      destiny: this.fromHome ? this.destination : this.origin,
    });
    this.distance = res.data.direction.distance / 1000 + " km";
    this.liters = res.data.finance.consumed;
    this.price = res.data.finance.price;
    this.wanted = res.data.finance.wanted;
    this.cost = res.data.finance.cost;
    this.pointOne = res.data.end.location;
    this.pointTwo = res.data.start.location;
    this.enquiryId = res.data.id;
  }
}
interface LatLng {
  lat: number | undefined;
  lng: number | undefined;
}
</script>

<style scoped lang="scss">
.n-card {
  margin: 10px auto;
  max-width: 300px;
  text-align: left;
}
.n-input {
  margin: 0.3rem 0px;
}
.n-switch {
  padding: 0.7rem 0.2rem;
}


.abx {
  width: 100%;
}

</style>
