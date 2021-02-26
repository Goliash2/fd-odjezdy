<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>
          <ion-grid style="padding: 0">
            <ion-row>
              <ion-col size="3" style="padding: 0">
                <img alt="logo FD" src="/odjezdy/horska/assets/img/logo_FD_cz.jpg" style="height: 50px; padding-top: 10px">
                <img alt="logo OI" src="/odjezdy/horska/assets/img/logo_OI.png" style="height: 50px; padding-top: 10px">
              </ion-col>
              <ion-col size="7" style="padding: 0;">
                <ion-title mode="ios" style="font-size: xx-large">ODJEZDY TRAMVAJÍ <small>(včetně zpoždění)</small></ion-title>
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
            <DeparturesBox :cis-id="58936" platform="A" station-name-comment="(směr centrum)"/>
            <DeparturesBox :cis-id="62891" platform="B" station-name-comment="(směr Nusle)"/>
            <ion-button mode="ios" expand="block" strong="true" size="large" href="https://dashboard.fd.cvut.cz">Vstoupit do infosystému FD</ion-button>
          </ion-col>
        </ion-row>
      </ion-grid>
    </ion-content>
    <ion-footer>
      <ion-toolbar fill="solid" color="primary" style="padding: 0 10px 0 10px; min-height: unset">
        <ion-text mode="ios">&copy; 2017-2021 Petr Hnyk, ČVUT v Praze, Fakulta dopravní, Ústav dopravní telematiky (K620), studentský projekt 20X1Y (oi.fd.cvut.cz). Data z datové platformy Golemio, bez záruky.</ion-text>
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
  components: {CurrentTime, DeparturesBox, MapContainer, IonHeader, IonToolbar, IonTitle, IonContent, IonPage, IonCol, IonGrid, IonRow, IonText, IonButton }
}
</script>

<style>
ion-toolbar {
  --min-height: 30px;
}
</style>
