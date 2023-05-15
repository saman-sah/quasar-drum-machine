<template>
  <q-page>
    <q-header elevated>
      <q-card class="my-card col">
        <q-card-section class="bg-purple text-white">
          <div class="text-h6">Drum Machine</div>
        </q-card-section>
      </q-card> 
    </q-header>
    <div class="q-pa-md row flex">
      <div class="col">
        <q-card class="my-card flex justify-between q-pa-sm">
          <q-toggle
          v-model="power"
          checked-icon="check"
          color="purple"
          label="Power"
          unchecked-icon="clear"
          />
          <q-btn @click="stopAudio()" color="black" label="Stop" />
          <q-btn style="background: #FF0080; color: white" label="No Sound" />
        </q-card>
        <q-card class="my-card row  q-pa-sm q-mt-sm">
          <q-toggle
          class="col-4"
          v-model="loop"
          checked-icon="check"
          color="purple"
          label="Loop"
          unchecked-icon="clear"
          />
          <q-slider
          class="col-8"
          v-model="volume"
          :step="1"
          :min="0" 
          :max="5"
        />
        </q-card>
        
      </div>
    </div>

    <div class="q-pa-md row flex">
      <div class="col">
        <q-card class="my-card flex justify-between q-pa-sm ">
          <q-btn
            class="q-ma-sm sound-btn"
            :style="{'cursor':power ? 'pointer' : 'not-allowed'}"
            v-for="sound in sounds"
            :key="sound.name"
            size="35px"
            round
            :color="power ? 'purple' : 'grey'"
            :label="sound.name"
            @click="playAudio(sound.url)"
          />
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'
import sounds from '../sounds.js'
export default defineComponent({
  name: 'IndexPage',
  data() {
    return {
      power: true,
      sounds,
      currenAudio:{},
      volume: 3,
      loop: true
    }
  },
  methods: {
    playAudio(url) {
      if (this.power && Object.keys(this.currenAudio).length === 0) {
        var audio = new Audio(url);
        audio.volume=this.volume / 5;
        audio.loop= this.loop;
        this.currenAudio=audio, 
        this.currenAudio.play();
        this.power=false
        this.currenAudio.addEventListener('ended', ()=> {
          console.log('end');
          this.power= true
        })
      }      	
    },
    stopAudio() {
      if(this.currenAudio) {
        console.log('stop audio');
        this.currenAudio.pause();	
      }
    }
  },
})
</script>
<style>
.nav-top {
    justify-content: space-between;
    display: flex;
    max-height: 50px !important;
}
.sound-btn .block{
  font-size: 16px
}
</style>
