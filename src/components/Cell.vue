<template>
  <div class="cell" 
    :class="{
        'cell--past': isPast, 
        'cell--weekend': isWeekend,
        'cell--today': isToday, 
        }">
        {{moment(date).format('D')}}
  </div>
</template>

<script>
import { computed } from 'vue'

import moment from 'moment'

export default {
    props: {
        date: {
            type: String,
            required: true,
        },
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
        padding: 1em;
        background: #FFF;
        border: 1px solid #EEE;
        border-radius: 10px;
        font-weight: bold;
        font-size: 20px;
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
</style>