<template>
  <div id="app">
    <section>
      Transition mode:
      <label>
        <input type="radio" :value="undefined" v-model="transitionMode"> undefined
      </label>

      <label>
        <input type="radio" value="in-out" v-model="transitionMode"> in-out
      </label>

      <label>
        <input type="radio" value="out-in" v-model="transitionMode"> out-in
      </label>
    </section>

    <div class="box">
      <font-awesome-icon icon="lock"/>
      <transition name="fade" :mode="transitionMode">
        <!-- <transition name="fade" mode="in-out"> -->
        <font-awesome-icon icon="spinner" spin class="spinner" v-if="loading"/>
      </transition>
    </div>
    <button v-on:click="toggleLoading()">Toggle</button>
    <button v-on:click="doubleToggle()">Double Toggle Sync</button>
    <button v-on:click="doubleToggleMixed()">Double Toggle Mixed</button>
    <button v-on:click="doubleToggleAsync()">Double Toggle Async</button>
    <button v-on:click="doubleToggleAsyncSlow()">Double Toggle Async Slow</button>
  </div>
</template>

<script>
import Vue from "vue";
import { library } from "@fortawesome/fontawesome-svg-core";
import { faLock, faSpinner } from "@fortawesome/free-solid-svg-icons";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

library.add(faLock, faSpinner);

Vue.component("font-awesome-icon", FontAwesomeIcon);

Vue.config.productionTip = false;

function sleep(n) {
  return new Promise(resolve => {
    setTimeout(resolve, n);
  });
}

export default {
  data() {
    return {
      loading: false,
      transitionMode: undefined
    };
  },
  methods: {
    toggleLoading() {
      const oldVal = this.loading;
      const newVal = !this.loading;
      console.log(`${oldVal} -> ${newVal}`); // eslint-disable-line no-console
      this.loading = !this.loading;
    },
    doubleToggle() {
      this.toggleLoading();
      this.toggleLoading();
    },
    doubleToggleMixed() {
      this.toggleLoading();
      sleep(0).then(this.toggleLoading);
    },
    async doubleToggleAsync() {
      await sleep(0);
      this.toggleLoading();
      await sleep(0);
      this.toggleLoading();
    },
    async doubleToggleAsyncSlow() {
      await sleep(0);
      this.toggleLoading();
      await sleep(50);
      this.toggleLoading();
    }
  }
};
</script>

<style>
.box {
  position: relative;
  display: inline-block;
  border: 1px solid red;
  padding: 0.5em;
}

.spinner {
  position: absolute;
  right: 0.125em;
  top: 0.125em;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}

button {
  margin: 0.25em;
}

label {
  display: block;
}

section {
  padding-bottom: 1em;
}
</style>
