<template>
  <div class="cell" 
    :class="{
        'cell--past': isPast, 
        'cell--weekend': isWeekend,
        'cell--today': isToday, 
        }">
        <div class="date">{{moment(date).format('D')}}</div>
        <div v-if="events" class="events">
            <Event 
                v-for="event in events" 
                :event="event"
                :key="event.id"
            />
        </div>
  </div>
</template>

<script>
import { computed } from 'vue'

import Event from './Event'

import moment from 'moment'

export default {
    components: { Event },
    props: {
        date: {
            type: String,
            required: true,
        },
        events: {
            type: Array,
            required: false,
        }
    },
    setup(props) {
        const isToday = computed(() => {
            return moment(props.date).format("YYYYMMDD") == moment().format("YYYYMMDD");
        }) 

        const isPast = computed(() => {
            return moment(props.date).format("YYYYMMDD") < moment().format("YYYYMMDD");
        }) 

        const isWeekend = computed(() => {
            return moment(props.date).format('dd') == 'сб' ||
                    moment(props.date).format('dd') == 'вс';
        }) 

        return {
            isToday,
            isPast,
            isWeekend,
            moment,
        }
    }
}
</script>

<style>
    .cell{
        min-width: 0;
        padding: 1em;
        background: #FFF;
        border: 1px solid #EEE;
        border-radius: 10px;
        font-weight: bold;
        font-size: 20px;
    }

    .date{
        display: flex;
        align-items: flex-start;
        justify-content: flex-end;
    }

    .cell--past{
        background: #EEE;
        color: #AAA;
    }

    .cell--weekend{
        color: #8E6CFF;
    }

    .cell--today{
        color: green;
        border-color: #CCC;
    }

    .events{
        text-align: left;
    }

    @media screen and (max-width: 500px){
        .cell{
            padding: .5em;
            font-size: 16px;
        }
    }
</style>