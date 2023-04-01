<template>
  <main-screen v-if="satusMatch == 'default'" @onStart="onHandleBeforeStart($event)"/>
  <interact-screen 
    v-if="satusMatch == 'match'"
    :cardContext = "setting.cardContext"
    @onFinish="onGetResult()"
  />
  <result-screen 
  v-if="satusMatch == 'result'" 
  :timer="timer"
  @onStartAgain="satusMatch = 'default'"
  />
  <copy-right-screen-vue/>
 
</template>

<script>
import MainScreen from './components/MainScreen.vue';
import InteractScreen from './components/InteractScreen';
import ResultScreen from './components/ResultScreen.vue';
import {shuffed} from "./utils/array.js";
import CopyRightScreenVue from './components/CopyRightScreen.vue';

export default {
 
  name: 'App',
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRightScreenVue,
  },
  data() {
    return {
      setting: {
        totalOfBlocks: 0,
        cardContext: [],
        startedAt: null,
      },
      satusMatch: 'default',
      timer: 0,
    };
  },
  
  methods: {
    onHandleBeforeStart(config) {
      //data ready
      this.setting.totalOfBlocks = config.totalOfBlocks;
      const firstCards = Array.from({length: this.setting.totalOfBlocks / 2}, (_, i) => i + 1);
    // array 1
      const secondFirstCards = [... firstCards];
    // array 2 use ES6
      const  cards = [...firstCards, ... secondFirstCards];
      // gop thanh 1 array use ES6
      this.setting.cardContext = shuffed(shuffed(shuffed(shuffed(cards))));
      // console.log(cards)
      this.setting.startedAt = new Date().getTime();
      this.satusMatch = "match";

    },
    onGetResult() {
      //get time
        this.timer = new Date().getTime() - this.setting.startedAt;
        // switch to result component
        this.satusMatch = "result";
    },
  }
}
</script>

