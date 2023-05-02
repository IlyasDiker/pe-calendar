<script setup>
import CalendarUi from "@/components/CalendarUi.vue"
import moment from "moment"
import {ref} from "vue"

const myEvents = ref([
  {
    startDateTime: moment(), 
    endDateTime: moment().add(1, 'hour'),
    title:"Weekly Followup"
  }
])
</script>

<template>
  <div class="w-screen h-screen">
    <CalendarUi :events="myEvents"/>
  </div>
</template>
