<template>
    <div class="bg-white shadow-sm overflow-hidden rounded flex flex-col h-[var(--event-height)] w-full border card-size border-blue-500" ref="EventCard" :style="getStyles()">
        <span class="h-2 bg-blue-500"></span>
        <div class="p-2">
            <span>{{ event.title }}</span>
        </div>
    </div>
</template>

<script>
import moment from 'moment';
import interact from 'interactjs'

let position = {x:0, y:0}

export default {
    data() {
        return {
            dragStarted: false,
            interactInstance: null,
            verticalMoveOffset: 0,
        }
    },
    mounted(){
        this.interactInstance = interact(this.$refs.EventCard).draggable({
            lockAxis: 'y',
            listeners: {
                start: (event) => {
                    console.log(event.type, event.target)
                },
                move: (event) => {
                    this.verticalMoveOffset += this.convertPixelToMinutes(event.dy);
                    // position.x += event.dx
                    // position.y += event.dy

                    // event.target.style.transform = `translate(${position.x}px, ${position.y}px)`
                },
                end: (event) => {
                    console.log('%cEventCard.vue line:40 this,verticalMoveOffset', 'color: #007acc;', this.verticalMoveOffset);
                    this.verticalMoveOffset = 0
                    this.$emit('onTimeChange');
                }
            }
        })
        // .resizable({
        //     edges:{
        //         bottom: true,
        //         top: true
        //     },
        //     listeners:{
        //         move: function (event){
        //             let { x, y } = event.target.dataset

        //             x = (parseFloat(x) || 0) + event.deltaRect.left
        //             y = (parseFloat(y) || 0) + event.deltaRect.top

        //             Object.assign(event.target.style, {
        //                 width: `${event.rect.width}px`,
        //                 height: `${event.rect.height}px`,
        //                 transform: `translate(${x}px, ${y}px)`
        //             })

        //             Object.assign(event.target.dataset, { x, y })
        //         }
        //     }
        // });
    },
    methods: {
        getStyles() {
            let hourStart = moment.duration(this.event.startDateTime.diff(this.event.startDateTime.clone().startOf('day'))).asMinutes() + this.verticalMoveOffset;
            let duration = moment.duration(this.event.endDateTime.diff(this.event.startDateTime)).asMinutes();
            return `--time-start-hour:${hourStart / 60}; --time-duration-hour:${duration / 60}`
        },
        convertPixelToMinutes(pixels){
            return pixels * this.$parent.hourBlockHeight
        }
    },
    props: {
        event: Object
    }
}
</script>

<style lang="scss" scoped>
.card-size {
    @apply absolute;
    top: calc(var(--hour-block-height)*var(--time-start-hour));
    height: calc(var(--time-duration-hour) * var(--hour-block-height));
}
</style>