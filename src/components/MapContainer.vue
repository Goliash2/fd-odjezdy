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
            <l-control-attribution position="bottomright" :prefix="mapyCzLogoUrl"></l-control-attribution>
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
      interval: null,
      mapyCzLogoUrl: '<img alt="mapy.cz logo" src="'+process.env.VUE_APP_BASE_URL+'/assets/logo/mapy-cz/mapy_logo.png" height=30>'
    };
  },
  methods: {
    log(a) {
      console.log(a);
    },
    getIconUrl(type) {
      return process.env.VUE_APP_BASE_URL+"/assets/icon/"+type+"-bubble-xs.png";
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
    urlStart.value = 'https://api.golemio.cz/v2/vehiclepositions?limit=1000&offset=0&routeShortName[]=1&routeShortName[]=2&routeShortName[]=3&routeShortName[]=4&routeShortName[]=5&routeShortName[]=6&routeShortName[]=7&routeShortName[]=8&routeShortName[]=9&routeShortName[]=10&routeShortName[]=11&routeShortName[]=12&routeShortName[]=13&routeShortName[]=14&routeShortName[]=15&routeShortName[]=16&routeShortName[]=17&routeShortName[]=18&routeShortName[]=19&routeShortName[]=20&routeShortName[]=21&routeShortName[]=22&routeShortName[]=23&routeShortName[]=24&routeShortName[]=25&routeShortName[]=26&routeShortName[]=27&routeShortName[]=28&routeShortName[]=29&routeShortName[]=30&routeShortName[]=31&routeShortName[]=32&routeShortName[]=148&routeShortName[]=176&routeShortName[]=91&routeShortName[]=92&routeShortName[]=93&routeShortName[]=94&routeShortName[]=95&routeShortName[]=96&routeShortName[]=97&routeShortName[]=98&routeShortName[]=99&routeShortName[]=207&routeShortName[]=135&routeShortName[]=909&routeShortName[]=908&routeShortName[]=175&routeShortName[]=133&routeShortName[]=194';
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
    const interval = setInterval(() => {
      urlStart.value = 'https://api.golemio.cz/v2/vehiclepositions?limit=1000&offset=0&routeShortName[]=1&routeShortName[]=2&routeShortName[]=3&routeShortName[]=4&routeShortName[]=5&routeShortName[]=6&routeShortName[]=7&routeShortName[]=8&routeShortName[]=9&routeShortName[]=10&routeShortName[]=11&routeShortName[]=12&routeShortName[]=13&routeShortName[]=14&routeShortName[]=15&routeShortName[]=16&routeShortName[]=17&routeShortName[]=18&routeShortName[]=19&routeShortName[]=20&routeShortName[]=21&routeShortName[]=22&routeShortName[]=23&routeShortName[]=24&routeShortName[]=25&routeShortName[]=26&routeShortName[]=27&routeShortName[]=28&routeShortName[]=29&routeShortName[]=30&routeShortName[]=31&routeShortName[]=32&routeShortName[]=148&routeShortName[]=176&routeShortName[]=91&routeShortName[]=92&routeShortName[]=93&routeShortName[]=94&routeShortName[]=95&routeShortName[]=96&routeShortName[]=97&routeShortName[]=98&routeShortName[]=99&routeShortName[]=207&routeShortName[]=135&routeShortName[]=909&routeShortName[]=908&routeShortName[]=175&routeShortName[]=133&routeShortName[]=194';
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
