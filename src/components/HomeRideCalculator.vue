<template>
  <div @keydown.enter="calculateThis()">
    <input
      type="checkbox"
      name="hey"
      v-model="fromHome"
      placeholder="ahj"
      id="a"
    />
    Z Černošic <br />
    <br /><br />

    <input v-if="!fromHome" type="text" name="Origin" placeholder="Místo odjezdu" v-model="destination" /> <br />

    <select name="Destiny" id="destin" v-model="origin">
      <option value="Brusinkova 1974, Cernosice">Brusinková 1974</option>
      <option value="Mokropeska 1713, Cernosice">Mokropeská</option>
    </select>
    <br />
    <input v-if="fromHome" type="text" name="Origin" placeholder="Místo doručení" v-model="destination" /> <br />


    <div>
      <ul>
        <li>
          Distance <b>{{ distance }}</b>
        </li>
        <li>
          Liters <b>{{ liters }}</b>
        </li>
          Wanted liter
        <b> {{ wanted }} </b>
        <li>
          Price <b>{{ price }}</b>
        </li>
        <li>
          Cost <b>{{ cost }}</b>
        </li>
      </ul>
    </div>
    <MapComponents :points="[pointOne, pointTwo]"></MapComponents>
  </div>
</template>

<script lang="ts">
import { Vue, Options } from "vue-class-component";
import axios from "axios";
import MapComponents from "./MapComponents.vue";


@Options({
  components: {
    MapComponents,
  }
})
export default class Calculation extends Vue {
  destination = "";
  origin = "Brusinkova 1974, Cernosice";

  fromHome = true;

  distance = "";
  liters = 0;
  wanted = 0;
  price = 0;
  cost = 0;

  pointOne= { lat: undefined, lng: undefined };
  pointTwo= { lat: undefined, lng: undefined };


  async calculateThis() {
    const res = await axios.post("http://localhost:3000/enquiry", {
      origin: this.origin,
      destiny: this.destination,
    });
    this.distance = res.data.direction.distance / 1000 + "km";
    this.liters = res.data.finance.consumed;
    this.price = res.data.finance.price;
    this.wanted = res.data.finance.wanted;
    this.cost = res.data.finance.cost;
    this.pointOne = res.data.end.location
    this.pointTwo = res.data.start.location

  }
}
</script>
