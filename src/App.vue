<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-12">
        <!-- Anfang: Template für die Calendar-Week-Component -->        
        <keep-alive>
          <transition name="fade" mode="out-in" appear>
            <component :is="activeView" />
          </transition>          
        </keep-alive>
        <!-- Ende: Template für die Calendar-Week-Component -->
      </div>
    </div>
    <div class="container-fluid row mt-3">
      <div class="col-4 offset-4">
        <!-- Anfang: Template für die Calendar-Entry-Component -->
        <CalendarEntry />
        <!-- Ende: Template für die Calendar-Day-Component -->
      </div>
      <div class="col-2 offset-2">
        <div class="float-end">
          <!-- Mit dem Button blenden wir die Calendar-Settings-Component ein bzw. aus. -->
          <button
            class="btn btn-lg mb-2"
            :class="buttonSettingsClasses"
            @click="toggleDisplaySettings()"
          >
            <i class="fas fa-cogs"></i>
          </button>
        </div>
        <!-- Anfang: Template für die Calendar-Settings-Component -->
        <transition name="fade">
          <CalendarSettings v-if="displaySettings" />
        </transition>
        <!-- Ende: Template für die Calendar-Day-Component -->
      </div>
    </div>
  </div>
</template>

<script>

import store from "./store";
import CalendarWeek from "@/components/CalendarWeek.vue";
import CalendarWeekAsList from "./components/CalendarWeekAsList.vue";
import CalendarEntry from "@/components/CalendarEntry.vue";
import CalendarSettings from "@/components/CalendarSettings.vue";

export default {
  name: "App",
  components: {
    CalendarWeek,
    CalendarWeekAsList,
    CalendarEntry,
    CalendarSettings,
  },
  data() {
    return {
      displaySettings: false,
    };
  },
  methods: {
    toggleDisplaySettings() {
      this.displaySettings = !this.displaySettings;
    },
  },
  computed: {
    buttonSettingsClasses() {
      return this.displaySettings ? ["btn-success"] : ["btn-outline-success"];
    },
    activeView() {
      return store.getters.activeView();
    },
  },
};
</script>

<style lang="scss">
@import "../node_modules/bootstrap/dist/css/bootstrap.min.css";
@import "../node_modules/@fortawesome/fontawesome-free/css/all.css";

body,
html {
  width: 100%;
  height: 100%;
  background-color: $second;
  font-size: 10px;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

.square {
  width: 4rem;
  height: 4rem;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.fade-enter-to,
.fade-leave-from {
  opacity: 1;
}

.fade-enter-active,
.fade-leave-active {
  transition: all 0.25s ease-out;
}

</style>
