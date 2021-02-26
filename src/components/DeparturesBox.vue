<template>
  <ion-list-header mode="ios">
    <ion-label v-if="!loading" mode="ios" style="font-size: x-large">{{ resp[0].stop.name }} <small>{{ stationNameComment }}</small>  <ion-spinner style="float: right" color="primary" name="circles" v-if="reloading"></ion-spinner></ion-label>
  </ion-list-header>
  <ion-list v-if="!loading">
    <ion-item v-for="(departure, index) in resp" :key="index" v-show="(departure.stop.platform_code===platform)">
      <ion-grid style="padding: 0">
        <ion-row>
          <ion-col size="1" style="padding: 0 15px 0 0; text-align: right;">
            <ion-label mode="ios" style="font-size: x-large">{{ departure.route.short_name }}</ion-label>
          </ion-col>
          <ion-col size="8" style="padding: 0;">
            <ion-label mode="ios" style="font-size: x-large">{{ departure.trip.headsign }}</ion-label>
          </ion-col>
          <ion-col size="3" style="padding: 0; text-align: right; vertical-align: text-bottom;">
            <ion-label mode="ios" style="font-size: x-large">
              <small>za</small>
              {{ hoursMinutes(departure.departure_timestamp.predicted) }}
              <small>min.</small></ion-label>
          </ion-col>
        </ion-row>
      </ion-grid>
    </ion-item>
  </ion-list>
  <ion-list v-if="loading">
    <ion-item>
      <ion-label mode="ios" style="font-size: x-large">Načítám odjezdy...</ion-label>
    </ion-item>
  </ion-list>
</template>

<script lang="ts">
import {
  IonItem,
  IonList,
  IonLabel,
  IonListHeader, IonCol, IonGrid, IonRow, IonSpinner
} from '@ionic/vue';
import { ref, onMounted } from 'vue'
import axios from 'axios';

export default {
  name: 'DeparturesBox',
  props: {
    name: String,
    cisId: Number,
    platform: String,
    stationNameComment: String
  },
  components: {
    IonItem,
    IonList,
    IonLabel,
    IonListHeader,
    IonCol,
    IonGrid,
    IonRow,
    IonSpinner
  },
  data() {
    return {
      zoom: 16
    };
  },
  methods: {
    log(a) {
      console.log(a);
    },
    hoursMinutes(timestamp) {
      const now = new Date();
      const departure = new Date(timestamp);
      const minutes = ((departure.valueOf()-now.valueOf())/60000);
      return Math.floor(minutes);
    }
  },
  setup(props) {
    const resp = ref('');
    const loading = ref(true);
    const reloading = ref(false);
    const urlStart = ref('');
    onMounted(() => {
      urlStart.value = 'https://api.golemio.cz/v2/departureboards/?limit=10&offset=0&cisIds='+props.cisId+'&minutesBefore=0&minutesAfter=60&preferredTimezone=Europe%2FPrague&orderBySchedule=false&showAllRoutesFirst=false';
      axios.get(urlStart.value, {
        headers: {
          'X-Access-Token': process.env.VUE_APP_GOLEMIO_ACCESS_TOKEN,
          "Content-Type": "application/json",
        }
      }).then(response => {
            resp.value = response.data;
            loading.value = false;
          })
          .catch(function (error) {
            console.log(error);
          });
      const interval = setInterval(() => {
        reloading.value = true;
        urlStart.value = 'https://api.golemio.cz/v2/departureboards/?limit=10&offset=0&cisIds='+props.cisId+'&minutesBefore=0&minutesAfter=60&preferredTimezone=Europe%2FPrague&orderBySchedule=false&showAllRoutesFirst=false';
        axios.get(urlStart.value, {
          headers: {
            'X-Access-Token': process.env.VUE_APP_GOLEMIO_ACCESS_TOKEN,
            "Content-Type": "application/json",
          }
        }).then(response => {
          resp.value = response.data;
          reloading.value = false;
        })
            .catch(function (error) {
              console.log(error);
            });
      }, 20000);

    })
    return {
      resp,
      loading,
      reloading
    }
  }
}
</script>
