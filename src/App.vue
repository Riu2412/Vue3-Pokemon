<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <interact-screen
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult"
  />
  <result-screen
    v-if="statusMatch === 'result'"
    :timer="timer"
    @onStartAgain="statusMatch = 'default'"
  />
  <coppy-right />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import CoppyRight from "./components/CoppyRight.vue";

import { shuffed } from "./utils/array";

export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CoppyRight,
  },
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
    };
  },
  methods: {
    onHandleBeforeStart(config) {
      console.log("running  before start", config);
      this.settings.totalOfBlocks = config.totalOfBlocks;
      //Tạo mảng
      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      // rải mảng
      const secondCards = [...firstCards];
      //ghep manga
      const cards = [...firstCards, ...secondCards];
      this.settings.cardsContext = shuffed(shuffed(shuffed(shuffed(cards))));
      //tinh time
      this.settings.startedAt = new Date().getTime();
      // data ready
      this.statusMatch = "match";
    },

    onGetResult() {
      //dua ra thoi gian hoan thanh
      this.timer = new Date().getTime() - this.settings.startedAt;

      // chuyen toi trang ket qua
      this.statusMatch = "result";
    },
  },
};
</script>
