<script>
import { IonGrid, IonCol, IonRow, IonButton } from '@ionic/vue'; 
import gameOptions from '../../public/assets/positions.json';

export default {
  name: 'Game',
  props: {
    mode: {
      type: String,
      require: true
    }
  },
  components: {
    IonGrid,
    IonCol,
    IonRow,
    IonButton
  },
  data() {
    return {
      timer: null,
      totalTime: (1 * 10),
      totalAmount: (5 * 60),
      finished: true,
      gameOptions: gameOptions,
      selectedOption: null
    }
  },
  computed: {
    minutes() {
      const minutes = Math.floor(this.totalTime / 60);
      return this.padTime(minutes);
    }, seconds() {
      const seconds = this.totalTime - (this.minutes * 60);
      return this.padTime(seconds);
    },
    allowShuffle() {
     return this.finished ? true : false;
    }
  },
  watch: {
    totalTime() {
      if(!this.totalTime) this.endGame();
    }
  },
  mounted() {
    this.setMatch();
  },
  methods: {
    start() {
      this.timer = setInterval(() => this.countdown(), 1000 );
      this.finished = false;
      if(this.finished) this.setMatch();
      this.reset = true;
    },
    setMatch() {
      const availableOptions = this.gameOptions.filter(x => x.mode === this.mode);
      this.selectedOption = availableOptions[Math.floor(Math.random() * availableOptions.length)];
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
      this.finished = true;
      this.timer = null;
    },
    endGame() {
      var audio = new Audio(require('../../public/assets/bell.mp3'));
      audio.play();
    },
    padTime(time) {
      return (time < 10 ? '0' : '') + time;
    }
  }
}
</script>

<template>
    <div id="container">
      <ion-grid>
      <ion-row class="ion-align-items-center">
        <ion-col>
          <h1 id="large">{{ minutes }}:{{ seconds }}</h1>
        </ion-col>
      </ion-row>
      <ion-row v-if="selectedOption">
        <ion-col>
          <h1>Start: {{ selectedOption.name }}</h1>
        </ion-col> <ion-col>
          <h1>Submission: {{ selectedOption.submission }}</h1>
        </ion-col>
      </ion-row>
      </ion-grid>
    </div>
    <div class="bottom">
      <ion-grid>
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
            v-if="allowShuffle"
            @click="setMatch"
            color="secondary"
            expand="block">
            Shuffle 
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
    </div>
</template>

<style scoped>
#container {
  text-align: center;
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}
#large {
  font-size: 45px;
}
.bottom {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
}
</style>
