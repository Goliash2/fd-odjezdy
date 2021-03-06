<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>
          <ion-grid style="padding: 0">
            <ion-row>
              <ion-col size="3" style="padding: 0">
                <a href="https://www.fd.cvut.cz"><img alt="logo FD" :src="logoFDurl" style="height: 50px; padding-top: 10px"></a>
                <a href="https://oi.fd.cvut.cz"><img alt="logo OI" :src="logoOIurl" style="height: 50px; padding-top: 10px"></a>
              </ion-col>
              <ion-col size="7" style="padding: 0;">
                <ion-title mode="ios" style="font-size: xx-large">ODJEZDY MHD <small>(včetně zpoždění)</small></ion-title>
              </ion-col>
              <ion-col size="2" style="padding: 0; text-align: right; vertical-align: text-bottom;">
                <CurrentTime />
              </ion-col>
            </ion-row>
          </ion-grid>
        </ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-grid class="h-100">
        <ion-row class="h-100">
          <ion-col size="6">
            <MapContainer name="MHD" />
          </ion-col>
          <ion-col size="6">
            <DeparturesBox :cis-id="58766" platform="" limit="7" walking-time="2" station-name-comment=" "/>
            <DeparturesBox :cis-id="27863" platform="" limit="7" walking-time="2" station-name-comment=" "/>
            <ion-button mode="ios" expand="block" strong="true" size="large" :href="dashboardUrl">Vstoupit do infosystému ČVUT FD</ion-button>
          </ion-col>
        </ion-row>
      </ion-grid>
    </ion-content>
    <ion-footer>
      <ion-toolbar fill="solid" color="primary" style="padding: 0 10px 0 10px; min-height: unset">
        <ion-text mode="ios">&copy; 2017-2021 Petr Hnyk, ČVUT v Praze, Fakulta dopravní, Ústav dopravní telematiky (K620), studentský projekt 20X1Y (<a href="https://oi.fd.cvut.cz" style="color: white">oi.fd.cvut.cz</a>). <span style="float: right">Ikony z grafiky PID, data z datové platformy Golemio, bez záruky.</span></ion-text>
      </ion-toolbar>
    </ion-footer>
  </ion-page>
</template>

<script lang="ts">
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonCol, IonGrid, IonRow, IonText, IonButton } from '@ionic/vue';
import MapContainer from '@/components/MapContainer.vue';
import DeparturesBox from "@/components/DeparturesBox.vue";
import CurrentTime from "@/components/CurrentTime.vue";

export default  {
  name: 'Departures',
  components: {CurrentTime, DeparturesBox, MapContainer, IonHeader, IonToolbar, IonTitle, IonContent, IonPage, IonCol, IonGrid, IonRow, IonText, IonButton },
  data() {
    return {
      logoFDurl: process.env.VUE_APP_BASE_URL+'/assets/img/logo_FD_cz.jpg',
      logoOIurl: process.env.VUE_APP_BASE_URL+'/assets/img/logo_OI.png',
      dashboardUrl: 'https://dashboard.fd.cvut.cz/?loc='+process.env.VUE_APP_LOC_STRING
    };
  },
}
</script>

<style>
ion-toolbar {
  --min-height: 30px;
}
</style>
