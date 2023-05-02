<template>
    <div class="w-full h-full flex flex-col">
        <div class="p-4 border-b flex flex-row justify-between">
            <div>

            </div>
            <div>
                <select v-model="selectedView" id="countries"
                    class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
                    <option value="day">Day</option>
                    <option value="week">Week</option>
                    <option value="month">Month</option>
                </select>
            </div>
        </div>

        <div class="flex-grow flex flex-col overflow-auto relative pl-10" style="--hour-block-height:40px">
            
            <nav class="flex flex-row w-full sticky top-0 bg-white/80 backdrop-blur z-20" :style="`--days-count:${ getViewDays().length };`">
                <template v-for="(item, index) in getViewDays()" :key="index">
                    <div class="flex flex-col justify-center items-center text-center flex-shrink-0 w-[calc(100%/var(--days-count))] border-b p-2 sticky">
                        <span>{{ item.format('dddd') }}</span>
                        <span class="aspect-square rounded-full w-8 flex justify-center items-center" :class="today.isSame(item, 'day')? 'bg-blue-500 text-white':''">{{ item.format('D') }}</span>
                    </div>
                </template>
            </nav>
            <div :class="{
                'inline-view': ['day', 'week'].includes(selectedView),
                'grid-view': selectedView == 'month'
            }" :style="`--days-count:${ getViewDays().length };`">
                <template v-for="(item, index) in getViewDays()" :key="index">
                    <div class="flex flex-col flex-shrink-0 border-l last:border-r h-[calc(var(--hour-block-height)*24)] w-[calc(100%/var(--days-count))] relative">
                        
                        <template v-if="events && events.length > 0">
                            <template v-for="(item, index) in getEventsForDay(item)" :key="index">
                                <EventCard :event="item"/>
                            </template>
                        </template>

                    </div>
                </template>

                <template v-if="['day', 'week'].includes(selectedView)">
                    <template v-for="(item) in 24" :key="item">
                        <div class="absolute top-[calc(var(--hour-span)*var(--hour-block-height))] -left-10 right-0 border-b h-0 -z-10 text-gray-400" :style="`--hour-span:${item}`">
                            <div class="relative">
                                <span class="absolute -translate-y-1/2 px-2 bg-gradient-to-r from-white to-white/50 w-10">{{ item }}</span>
                            </div>
                        </div>
                    </template>
                </template>
                <template v-else>
    
                </template>
            </div>
            
        </div>
    </div>
</template>

<script>
import moment from "moment"
import EventCard from "./EventCard.vue"

export default {
    data() {
        return {
            selectedView: 'week',
            today: moment()
        }
    },
    methods: {
        getViewDays() {
            let today = moment()
            switch (this.selectedView) {
                case 'day':
                    return [today];
                    break;
                case 'week':
                    return this.getCurrentTimeSpan('week');
                    break;
                case 'month':
                    return this.getCurrentTimeSpan('month');
                    break;

                default:
                    break;
            }
        },
        getCurrentTimeSpan(timeSpan) { // 'week', 'month', 'day'
            var currentDate = moment();

            var weekStart = currentDate.clone().startOf(timeSpan);
            var weekEnd = currentDate.clone().endOf(timeSpan);

            var days = [];

            for (var i = 0; i <= 6; i++) {
                days.push(moment(weekStart).add(i, 'days'));
            }
            return days
        },
        getEventsForDay(day){
            let events = this.events.filter(x => (x.startDateTime && x.startDateTime.isSame(day,'day')) || (x.endDateTime && x.endDateTime.isSame(day,'day')))
            return events
        }
    },
    props:{
        events: Array
    },
    setup() {

    },
    components:{
        EventCard
    }
}
</script>

<style lang="scss" scoped>
.inline-view {
    @apply flex flex-row w-full relative;
}

.grid-view {
    @apply grid grid-cols-7 relative;
}
</style>