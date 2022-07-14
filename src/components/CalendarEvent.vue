<template>
  <div class="alert text-center" :class="alertColor">
    <transition name="fade" mode="out-in">
      <!-- Tamplate für den Fall, dass das Event nicht bearbeitet wird -->
      <div v-if="!event.edit">
        <div>
          <!-- <strong>{{ priorityDisplayName }}</strong> -->
          <slot name="eventPriority" :priorityDisplayName="priorityDisplayName">
            <strong>{{ priorityDisplayName }}</strong>
          </slot>
        </div>

        <!-- <div>{{ event.title }}</div> -->
        <slot name="eventTitle" :event="event">
          <div>{{ event.title }}</div>
        </slot>
        <div>
          <i class="fas fa-edit me-2" role="button" @click="editEvent()"></i>
          <i class="far fa-trash-alt" role="button" @click="deleteEvent()"></i>
        </div>
      </div>
      <div v-else>
        <!-- 
          @input "one-way binding", manuelle verwaltung des inputs feldes, 
          besser: "two-way binding", wie bei der section!
          man würde sogar eine methode sparen!
        -->
        <input
          type="text"
          class="form-control"
          ref="newEventTitleInput"
          :placeholder="event.title"
          @input="setNewEventTitle($event)" 
        />
        <!-- 
          v-model "two-way binding", es übernimmt für uns beide richtungen 
          erspart uns das auswendig lernen von event-listeners! 
        -->
        <select class="form-select mt-2" v-model="newEventPriority" >
          <option value="-1">Hoch</option>
          <option value="0">Mittel</option>
          <option value="1">Tief</option>
        </select>
        <hr />
        <i class="fas fa-check" role="button" @click="updateEvent()"></i>
      </div>
    </transition>
  </div>

</template>

<script>
import store from "../store";

export default {
  name: "CalendarEvent",

  components: {
  },

  props: {
    event: Object,
    day: Object,

  },

  data() {
    return {
      newEventTitle: "",
      newEventPriority: this.event.priority,
    };

  },

  computed: {

    priorityDisplayName() {
      switch (this.event.priority) {
        case 1:
          return "Tief";
        case 0:
          return "Mittel";
        case -1:
          return "Hoch";
      }
      return "Unbekannte Priorität";
    },

    alertColor() {
      return "alert-" + this.event.color;
    },

  },

  methods: {

    deleteEvent() {
      store.mutations.deleteEvent(this.day.id, this.event.title);
    },

    editEvent() {      
      store.mutations.editEvent(this.day.id, this.event.title);
      // auf die template refs zugreifen
      this.$nextTick(() => {
        this.$refs.newEventTitleInput.focus();
      });
    },

    setNewEventTitle(event) {
      this.newEventTitle = event.target.value;
    },

    updateEvent() {
      store.mutations.updateEvent(
        this.day.id,
        this.event.title,
        { 
          title: this.newEventTitle,
          priority: this.newEventPriority
        }
      );
    },

  },

};
</script>

<style lang="sass" scoped>
@import '@/scss/variables.scss'
</style>
