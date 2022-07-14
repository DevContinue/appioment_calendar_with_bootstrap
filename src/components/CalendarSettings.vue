<template>

    <div id="calendar-settings">
          <div class="card">
            <div class="card-header text-center bg-success text-white">
              <strong>Einstellungen</strong>
            </div>
            <div class="card-body">
              <ul class="nav nav-pills nav-fill">
                <li 
                  v-for="(icon, componentName) in views" 
                  :key="componentName"  
                  class="nav-item" 
                  role="button">
                    <a class="nav-link" 
                      :class="isActiveView(componentName)" 
                      @click="changeActiveView(componentName)">
                        <i class="text-success" :class="icon"></i>
                    </a>
                </li>
              </ul>
              <hr />
              <ul class="nav nav-pills nav-fill">
                <li class="nav-item" role="button">
                  <a 
                    class="nav-link" 
                    :class="isActiveOrdering('priority')" 
                    @click="changeOrdering('priority')"
                    ><i class="fas fa-sort-numeric-down-alt text-success"></i
                  ></a>
                </li>
                <li 
                  class="nav-item" 
                  :class="isActiveOrdering('title')" 
                  @click="changeOrdering('title')"
                  role="button"
                >
                  <a class="nav-link"
                    ><i class="fas fa-sort-alpha-down text-success"></i
                  ></a>
                </li>
              </ul>
            </div>
          </div>
        </div>

</template>

<script>
import store from "../store.js";

export default {
    name: 'CalendarSettings',
    components: {


    },
    data() {
      return {
        views: {
          CalendarWeek: "fa fa-table",
          CalendarWeekAsList: "fas fa-list",
        },
      }
    },
    methods: {
      changeActiveView(componentName) {
        store.mutations.setActiveView(componentName);
      },

      isActiveView(componentName) {
        if (componentName === store.getters.activeView()) {
          return ["border border-success"]
        }
      },

      changeOrdering(ordering) {
        store.mutations.setActiveOrdering(ordering);
      },

      isActiveOrdering(ordering) {
        if (ordering === store.getters.activeOrdering()) {
          return ["border border-success"]
        }
      },

    }
}

</script>

<style lang='sass' scoped>
@import '@/scss/variables.scss'

.card
  background-color: #DCD7C9

  .card-header
      background-color: #A27B5C

</style>