<template>
    <div class="bg-white shadow-sm overflow-hidden rounded flex flex-col h-[var(--event-height)] w-full border card-size border-blue-500"
        :style="getStyles()">
        <span class="h-2 bg-blue-500"></span>
        <div class="p-2">
            <span>{{ event.title }}</span>
        </div>
    </div>
</template>

<script>
export default{
    methods:{
        getStyles(){
            console.log('%cEventCard.vue line:15 this.event', 'color: #007acc;', this.event);
            let hour = this.event.startDateTime.hour();
            let duration = this.event.endDateTime.hour() - this.event.startDateTime.hour()
            console.log('%cEventCard.vue line:16 hour', 'color: #007acc;', hour);
            return `--time-start-hour:${hour}; --time-duration-hour:${duration}`
        }
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