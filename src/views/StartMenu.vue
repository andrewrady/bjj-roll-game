<script>
import { IonButton } from '@ionic/vue';

export default {
  name: 'StartMenu',
  components: { IonButton },
  data() {
    return {
      difficulty: {
        easy: 'easy',
        medium: 'medium',
        hard: 'hard'
      }
    }
  }
}
</script>

<template>
  <div id="container">
    <ion-button 
      expand="block" 
      fill="solid" 
      @click="$emit('set-difficulty', difficulty.easy)">
      Easy
    </ion-button>
    <ion-button 
      expand="block" 
      fill="solid"
      @click="$emit('set-difficulty', difficulty.medium)">
      Medium
    </ion-button>
    <ion-button 
      expand="block" 
      fill="solid"
      @click="$emit('set-difficulty', difficulty.hard)">
      Hard
    </ion-button>
  </div>
</template>
