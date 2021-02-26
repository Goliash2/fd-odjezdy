<template>
  <ion-list v-if="loading">
    <ion-item>
      <ion-label mode="ios" style="font-size: x-large">Připravuji a načítám mapu...</ion-label>
    </ion-item>
  </ion-list>
<div class="map-div">
          <l-map
            v-if="!loading"
            v-model="zoom"
            v-model:zoom="zoom"
            :center="[location.coords.latitude, location.coords.longitude]"
        >
          <l-tile-layer
              url="https://m1.mapserver.mapy.cz/turist-m/{z}-{x}-{y}"
          ></l-tile-layer>
            <l-control-attribution position="bottomleft" prefix='&copy; <a href="http://www.seznam.cz">Seznam.cz, a.s.</a>, <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'></l-control-attribution>
            <l-control-attribution position="bottomright" prefix='<img src="/odjezdy/horska/assets/logo/mapy-cz/mapy_logo.png" height=30>'></l-control-attribution>
          <l-marker v-for="(vehicle) in resp.features"
          :key="vehicle.properties.id"
              :lat-lng="[vehicle.geometry.coordinates[1],vehicle.geometry.coordinates[0]]"
          >
            <l-icon class="extra-class" :icon-anchor="[20, 42]">
              <img :src="getIconUrl(vehicle.properties.trip.vehicle_type.description_en)">
              <div class="headline" style="position: relative; top: -46px; left: 13px; font-size: x-large; text-align: center;">
                {{ vehicle.properties.trip.gtfs.route_short_name }}
              </div>
            </l-icon>
          </l-marker>
        </l-map>
      </div>
</template>

<script lang="ts">
import { LMap, LTileLayer, LMarker, LIcon, LControlAttribution } from "@vue-leaflet/vue-leaflet";
import { ref, onMounted } from 'vue'
import "leaflet/dist/leaflet.css";
import axios from 'axios';

export default {
  name: 'MapContainer',
  props: {
    name: String
  },
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LIcon,
    LControlAttribution
  },
  data() {
    return {
      zoom: 16,
      interval: null
    };
  },
  methods: {
    log(a) {
      console.log(a);
    },
    getIconUrl(type) {
      return "/odjezdy/horska/assets/icon/"+type+"-bubble-xs.png";
    }
  },
  setup(name) {
    const resp = ref(0);
    const loading = ref(true);
    const location = ref({coords: {
      latitude: process.env.VUE_APP_STATION_LAT,
      longitude: process.env.VUE_APP_STATION_LNG
    }});
    const urlStart = ref('');
  onMounted(() => {
    urlStart.value = 'https://api.golemio.cz/v2/vehiclepositions?limit=1000&offset=0';
    axios.get(urlStart.value, {
      headers: {
        'X-Access-Token': process.env.VUE_APP_GOLEMIO_ACCESS_TOKEN,
        "Content-Type": "application/json",
      }
    })
        .then(response => {
          resp.value = response.data;
          loading.value = false;
          console.log(resp);
        })
        .catch(function (error) {
          console.log(error);
        });
    const interval = setInterval(() => {
      urlStart.value = 'https://api.golemio.cz/v2/vehiclepositions?limit=1000&offset=0';
           axios.get(urlStart.value, {
            headers: {
            'X-Access-Token': process.env.VUE_APP_GOLEMIO_ACCESS_TOKEN,
            "Content-Type": "application/json",
            }
        })
        .then(response => {
          resp.value = response.data;
          loading.value = false;
        })
       .catch(function (error) {
             console.log(error);
        });
           }, 10000);

    })
    return {
        resp,
        loading,
        location
      }
  }
}
</script>

<style scoped>
.map-div {
  height: 100%;
  width: 100%;
}
.extra-class {
   background-color: aqua;
   padding: 10px;
   border: 1px solid #333;
   border-radius: 0 20px 20px 20px;
   box-shadow: 5px 3px 10px rgba(0, 0, 0, 0.2);
   text-align: center;
   width: auto !important;
   height: auto !important;
   margin: 0 !important;
 }
</style>
