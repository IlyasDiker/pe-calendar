<template>
    <div class="bg-white shadow-sm overflow-hidden rounded flex flex-col h-[var(--event-height)] w-full border card-size border-blue-500" draggable="true"
        @dragstart="dragStart"
        :style="getStyles()">
        <span class="h-2 bg-blue-500"></span>
        <div class="p-2">
            <span>{{ event.title }}</span>
        </div>
    </div>
</template>

<script>
import moment from 'moment';
import interact from 'interactjs'

export default{
    methods:{
        getStyles(){
            let hourStart = +moment.duration(this.event.startDateTime.diff(this.event.startDateTime.clone().startOf('day'))).asMinutes();
            let duration = moment.duration(this.event.endDateTime.diff(this.event.startDateTime)).asMinutes()
            return `--time-start-hour:${hourStart/60}; --time-duration-hour:${duration/60}`
        },
        allowDrop(event) {
            event.preventDefault();
        },
        dragStart(e) {
            e.dataTransfer.setData("event", this.event);
        },
    },
    props:{
        event: Object
    }
}
</script>

<style lang="scss" scoped>
.card-size{
    @apply absolute;
    top: calc(var(--hour-block-height)*var(--time-start-hour));
    height: calc(var(--time-duration-hour) * var(--hour-block-height));
}
</style>