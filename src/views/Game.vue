<script>
import { IonGrid, IonCol, IonRow, IonButton } from '@ionic/vue'; 

export default {
  name: 'Game',
  components: {
    IonGrid,
    IonCol,
    IonRow,
    IonButton,
  },
  data() {
    return {
      timer: null,
      totalTime: (5 * 60),
      finished: false
    }
  },
  computed: {
    minutes() {
      const minutes = Math.floor(this.totalTime / 60);
      return this.padTime(minutes);
    }, seconds() {
      const seconds = this.totalTime - (this.minutes * 60);
      return this.padTime(seconds);
    }
  },
  methods: {
    start() {
      this.timer = setInterval(() => this.countdown(), 1000 );
      this.reset = true;
    },
    countdown() {
      if(this.totalTime >= 1) {
        this.totalTime--;
      } else {
        this.totalTime = 0;
        this.finished = true;
      }
    },
    pause() {
      clearInterval(this.timer);
      this.timer = null;
    },
    reset() {
      this.totalTime = (5 *60);
      clearInterval(this.timer);
      this.timer = null;
    },
    padTime(time) {
      return (time < 10 ? '0' : '') + time;
    }
  }
}
</script>

<template>
  <ion-grid>
    <ion-row class="ion-align-items-center">
      <ion-col>
        <h1>{{ minutes }}:{{ seconds }}</h1>
      </ion-col>
    </ion-row>
    <ion-row>
      <ion-col>
        <ion-button 
          v-if="!timer"
          @click="start"
          color="secondary"
          expand="block">
          Start
        </ion-button>
        <ion-button
          v-else
          @click="pause"
          color="secondary"
          expand="block">
          Pause
        </ion-button>
      </ion-col>
    </ion-row>
    <ion-row>
      <ion-col>
        <ion-button 
          @click="reset"
          color="secondary"
          expand="block">
          Reset
        </ion-button>
      </ion-col>
      <ion-col>
        <ion-button 
          color="secondary"  
          expand="block" 
          @click="$emit('cancel', null)">
          Cancel
        </ion-button>
      </ion-col>
    </ion-row>
  </ion-grid>
</template>
