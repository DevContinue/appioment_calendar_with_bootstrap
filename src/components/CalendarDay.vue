<template>
  <div class="card border-start" :class="cardClasses">
    <div
      class="card-header text-center"
      :class="cardHeaderClasses"
      role="button"
      @click="setActiveDay()"
    >
      <strong>{{ day.fullName }}</strong>
    </div>
    <div class="card-body">
      <Transition name="fade" mode="out-in">
        <div v-if="day.events.length">
          <!-- Anfang: Template für die Calendar-Event-Component -->
          <TransitionGroup name="list">
            <CalendarEvent
              v-for="event in events"
              :key="event.title"
              :event="event"
              :day="day"
            >
              <template #eventPriority="slotProps">
                <h5>{{ slotProps.priorityDisplayName }}</h5>
              </template>

              <template #eventTitle="{ event: entry }">
                <em>{{ entry.title }}</em>
              </template>
            </CalendarEvent>
          </TransitionGroup>
          <!-- Ende: Template für die Calendar-Event-Component -->
        </div>
        <div v-else>
          <div class="alert alert-light text-center">
            <em>Keine Termine</em>
          </div>
        </div>
      </Transition>
    </div>
  </div>
</template>

<script>
import CalendarEvent from "./CalendarEvent.vue";
import store from "../store";

export default {
  name: "CalendarDay",
  components: {
    CalendarEvent,
  },
  props: {
    day: {
      type: Object,
      required: true,
      default: function () {
        return {
          id: -1,
          fullName: "Fehlender Wochentag",
          events: [],
        };
      },
      validator: function (value) {
        if (Object.keys(value).includes("id")) {
          return true;
        }
      },
    },
  },
  methods: {
    setActiveDay() {
      store.mutations.setActiveDay(this.day.id);
    },
  },
  computed: {
    cardClasses() {
      return this.day.id === store.getters.activeDay().id
        ? ["border-primary"]
        : null;
    },

    cardHeaderClasses() {
      return this.day.id === store.getters.activeDay().id
        ? ["bg-primary", "text-white"]
        : null;
    },

    events() {
      return store.getters.events(this.day.id);
    },
  },
};
</script>

<style lang="sass" scoped>
@import '@/scss/variables.scss'

.card
  background-color: #DCD7C9

.alert-light
  background-color: #DCD7C9
  border: 0

.card-header
  background-color: #A27B5C

.list-enter-from,
.list-leave-to
  opacity: 0
  transform: translateY(30px)

.list-enter-to,
.list-leave-from
  opacity: 1
  transform: translateY(0)


.list-enter-active,
.list-leave-active
  transition: all 1s ease
</style>
