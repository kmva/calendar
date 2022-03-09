<template>
    <div class="calendar">
        <h1>
            <div class="arrow left" @click="prevMonth"></div>
                {{month}}
            <div class="arrow right" @click="nextMonth"></div>
            <span v-if="showYear" class="year">{{year}}г.</span>
        </h1>

        <div class="weekdays">
            <div class="weekday">Понедельник</div>
            <div class="weekday">Вторник</div>
            <div class="weekday">Среда</div>
            <div class="weekday">Четверг</div>
            <div class="weekday">Пятница</div>
            <div class="weekday">Суббота</div>
            <div class="weekday">Воскресенье</div>  
        </div>

        <div class="calendar-wrapper">
            <div class="cell"></div>
            <Cell 
                v-for="i in moment(date).daysInMonth()" 
                :key="i" 
                :date="moment(year + moment(date).format('MM') + i, 'YYYYMMD').format()"
                :events="events.filter(event => moment(event.date).format('MM DD YYYY') == moment(year + moment(date).format('MM') + i, 'YYYYMMD').format('MM DD YYYY'))"
            /> 
        </div>
    </div>
</template>

<script>
import { computed, ref, onMounted } from 'vue'

import Cell from './Cell'

import moment from 'moment'

export default {
    components: { Cell },
    props: {
        events: {
            type: Array,
            required: true,
        }
    },
    setup(){
        const showYear = computed(() => {
            return moment(date.value).format('YYYY') !== moment().format('YYYY');
        })

        const date = ref('')

        const month = computed(() => {
            return moment(date.value).format('MMMM');
        })

        const year = computed(() => {
            return moment(date.value).format('YYYY');
        })

        onMounted(() => {
            moment.locale('ru');
            date.value = moment().format();
        })


        function prevMonth() {
           date.value = moment(date.value).subtract(1, 'month');
        }

        function nextMonth() {
           date.value = moment(date.value).add(1, 'month');
        }

        return {
            showYear,
            moment,
            date,
            month, 
            year,

            prevMonth,
            nextMonth,
        }
    }
}
</script>

<style>
    body{
        background: #EEE;
    }

    .calendar{
        margin-top: 2em;
        background: #FFF;
        border-radius: 15px;
        padding: 1em 2em;
    }

    .calendar-wrapper{
        display: grid;
        grid-template-columns: repeat(7, 1fr);
        grid-auto-rows: minmax(150px, auto);
        gap: .3em;
        /* background: #EEE; */
        padding: 0
    }

    h1{
        display: flex;
        align-items: center;
        column-gap: .5em;
        text-align: left;
        text-transform: uppercase;
        color: #4900AD;
    }

    h1 button{
        background: none;
        border: none;
    }

    .arrow {
        border: solid black;
        border-width: 0 3px 3px 0;
        display: inline-block;
        padding: 3px;
        transition: .3s;
    }

    .arrow:hover, .arrow:active {
        cursor: pointer;
        border-color: #4900AD
    }

    .right {
        transform: rotate(-45deg);
        -webkit-transform: rotate(-45deg);
    }

    .left {
        transform: rotate(135deg);
        -webkit-transform: rotate(135deg);
    }

    .weekdays{
        display: grid;
        grid-template-columns: repeat(7, 1fr);
        margin: 2.5em 0 1em;
    }

    .weekday{
        text-transform: uppercase;
        font-weight: bold;
    }

    .year{
        margin-left: 1em
    }
</style>