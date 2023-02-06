<style>
@import 'bootstrap';
@import 'datatables.net-bs5';
</style>

<template>
  <div class="container" style="padding:20px">
    <div class="row">
      <div class="col-sm">
        <select v-model="currentNoc">
          <option v-for="c in countriesNoc">{{ c }}</option>
        </select>
      </div>
      <div class="col-sm">
        <select v-model="currentEvent">
          <option v-for="e in events">{{ e }}</option>
        </select>
      </div>
    </div>
    <div class="row" style="padding:20px">
      <medallsComponent :currentNoc="currentNoc" :currentEvent="currentEvent"></medallsComponent>
    </div>
    <div class="row" style="padding:20px">
      <eventComponent :currentNoc="currentNoc" :currentEvent="currentEvent"></eventComponent>
    </div>
    <div class="row" style="padding:20px">
      <ageComponent></ageComponent>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import medallsComponent from "./components/medallsComponent.vue";
import eventComponent from "./components/eventComponent.vue";
import ageComponent from "./components/ageComponent.vue";

export default {
  data() {
    return {
      countries: [],
      countriesNoc: [],
      events: [],
      currentEvent: "Alpine Skiing Men's Combined",
      currentNoc: "AUT"
    }
  },
  methods: {
    async getContries() {
      await axios.get(`http://127.0.0.1:5000/regions`)
        .then(result => this.countries = result.data)
    },
    async getEvents() {
      await axios.get(`http://127.0.0.1:5000/get_events_by_NOC/${this.currentNoc}`)
        .then(result => this.events = result.data)
    }
  }, async mounted() {
    await this.getContries()
    await this.getEvents()
    this.countries.forEach(element => {
      this.countriesNoc.push(element.noc)
    });

  }, watch: {
    currentNoc() {
      this.getEvents()
      e
    }
  },
  components: {
    medallsComponent,
    eventComponent,
    ageComponent
  }

}


</script>
