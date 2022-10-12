<template>
  <div>
    <input type="text" name="Origin" v-model="origin" />
    <input
      type="text"
      name="Destiny"
      v-model="destination"
      @keypress.enter="calculateThis()"
    />

    <div>
      <ul>
        <li>
          Distance <b>{{ distance }}</b>
        </li>
        <li>
          Liters <b>{{ liters }}</b>
        </li>
        Wanted liter
        <b> {{ wanted }}</b>
        <li>
          Price <b>{{ price }}</b>
        </li>
        <li>
          Cost <b>{{ cost }}</b>
        </li>
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
import { Vue } from "vue-class-component";
import axios from "axios";

export default class Calculation extends Vue {
  origin = "";
  destination = "";

  distance = 0;
  liters = 0;
  wanted = 0;
  price = 0;
  cost = 0;

  async calculateThis() {
    const res = await axios.post("http://localhost:3000/routekm", {
      origin: this.origin,
      destiny: this.destination,
    });
    this.distance = res.data.distance;
    this.liters = res.data.consumed;
    this.price = res.data.price;
    this.wanted = res.data.wanted;
    this.cost = res.data.cost;
  }
}
</script>
