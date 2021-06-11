<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Game of Rolls</ion-title>
      </ion-toolbar>
    </ion-header>
    
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Blank</ion-title>
        </ion-toolbar>
      </ion-header>
    
      <div> 
        <start-menu 
          @set-difficulty="setDifficulty" 
          v-if="!difficulty" />
        <game-cmp 
          v-else
          :mode="difficulty"
          @cancel="setDifficulty"/>
      </div>
    </ion-content>
  </ion-page>
</template>

<script>
import StartMenu from './StartMenu.vue';
import GameCmp from './Game.vue';
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar } from '@ionic/vue';
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'Home',
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    StartMenu,
    GameCmp
  },
  data () {
    return {
      difficulty: null
    }
  },
  methods: {
    setDifficulty(value) {
      this.difficulty = value;
    }
  }
});
</script>

