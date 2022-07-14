<template>

        <div id="calender-entry">
          <div class="card">
            <div class="card-header text-center">
              <h5>Neuer Termin für: <strong>{{ activeDay }}</strong></h5>
            </div>
            <div class="card-body">
              <div class="alert alert-danger" v-if="error">
                Der Title darf nicht leer sein.
              </div>
              <input
                type="text"
                class="form-control"
                placeholder="Neuer Eintrag"
                ref="eventTitleInput"
                v-model="event.title"
                @keyup.enter.exact="submitEvent()"
                @keyup.ctrl.enter.exact="resetEventTitle()"
              />
              <select class="form-select mt-2" v-model="event.priority">  
                <option value="-1">Hoch</option>
                <option value="0">Mittel</option>
                <option value="1">Tief</option>
              </select>
              <div class="text-center mt-3">
                <span
                  v-for="color in eventColors"
                  :key="color"
                  class="d-inline-block alert m-0 me-2 square"      
                  :class="eventColorClasses(color)"
                  @click="setEventColor(color)"  
                  role="button"
                >
                </span>
              </div>
              <hr />
              <div class="d-grid gap-2">
                <button class="btn btn-primary" :disabled="setDisabledStatus" @click="submitEvent()">Eintragen</button>
                <button class="btn btn-danger" @click="resetEventTitle()">Inhalt löschen</button>
              </div>
            </div>
          </div>
        </div>

</template>

<script>
import store from '../store';


export default {
    name: 'CalendarEntry',
    components: {
    },
    data() {
      return {
        eventColors: ["primary", "success", "info", "warning", "danger"],
        event: {
          title: "",
          color: "primary",
          priority: 0,
        },
        error: false,
      };
    },
    computed: {
      activeDay() {
        return store.getters.activeDay().fullName;
      },
      setDisabledStatus() {
        return this.event.title === "";
      },
    },

    mounted() {
      this.$refs.eventTitleInput.focus();
    },

    methods: {
      eventColorClasses(eventColor) {
        return [ 
          "alert-" + eventColor,
          this.event.color === eventColor
          ? "border border-" + this.event.color
          : "" 
        ];
      },

      resetEventTitle() {
        this.event.title = "";
      },

      setEventColor(eventColor) {
        this.event.color = eventColor;
      },

      setEventInput() {
        store.mutations.setEventInput(this.day.id);
      },

      submitEvent() {
        if(this.event.title === "" ) return (this.error = true); 
        store.mutations.storeEvent(this.event);
          this.$nextTick(() => {
            this.event = {
              title: "",
              color: "primary",
              priority: 0
            }
            
          this.error = false;
          });
      },
      
    },

};

</script>

<style lang='sass' scoped>
@import '@/scss/variables.scss'

.card
  background-color: #DCD7C9

  .card-header
      background-color: #A27B5C

</style>