<template>
    <div class="calendar">
        <h1>
            <div class="arrow left" @click="toPrevMonth"></div>
                {{month}}
            <div class="arrow right" @click="toNextMonth"></div>
            <span v-if="showYear" class="year">{{year}}г.</span>
        </h1>
        <div class="weekdays">
            <div class="weekday"><div class="long">Понедельник</div><div class="short">ПН</div></div>
            <div class="weekday"><div class="long">Вторник</div><div class="short">ВТ</div></div>
            <div class="weekday"><div class="long">Среда</div><div class="short">СР</div></div>
            <div class="weekday"><div class="long">Четверг</div><div class="short">ЧТ</div></div>
            <div class="weekday"><div class="long">Пятница</div><div class="short">ПТ</div></div>
            <div class="weekday"><div class="long">Суббота</div><div class="short">СБ</div></div>
            <div class="weekday"><div class="long">Воскресенье</div><div class="short">ВС</div></div>  
        </div>

        <div class="calendar-wrapper">
            <!-- Дни прошлого месяца -->
            <Cell 
                v-for="i in remainingDaysOfPrevMonth"
                :key="i"
                :date="moment(prevMonth).set('date', i)"
                :events="events.filter(event => moment(event.date).format('YYYYMMD') == moment(prevMonth).set('date', i).format('YYYYMMD'))"
            />

            <!-- Дни текущего месяца -->
            <Cell 
                v-for="i in moment(date).daysInMonth()" 
                :key="i" 
                :date="moment(year + moment(date).format('MM') + i, 'YYYYMMD').format()"
                :events="events.filter(event => moment(event.date).format('YYYYMMD') == moment(date).set('date', i).format('YYYYMMD'))"
            /> 

            <!-- Дни следующего месяца -->
            <Cell 
                v-for="i in 7 - lastWeekdayOfCurrentMonth"
                :key="i"
                :date="moment(nextMonth).set('date', i)"
                :events="events.filter(event => moment(event.date).format('YYYYMMD') == moment(nextMonth).set('date', i).format('YYYYMMD'))"
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

        const firstWeekdayOfCurrentMonth = computed(() => {
            return moment(date.value).startOf('month').isoWeekday();
        })
        const lastWeekdayOfCurrentMonth = computed(() => {
            return moment(date.value).endOf('month').isoWeekday();
        });

        const prevMonth = computed(() => {
            return moment(moment(date.value).subtract(1, 'month'));
        });
        const nextMonth =  computed(() => {
            return moment(moment(date.value).add(1, 'month'));
        });
            
        const prevMonthDays = computed(() => {
            return prevMonth.value.daysInMonth();
        });

        const nextMonthDays = computed(() => {
            return nextMonth.value.daysInMonth();
        });

        const month = computed(() => {
            return moment(date.value).format('MMMM');
        })

        const year = computed(() => {
            return moment(date.value).format('YYYY');
        })

        onMounted(() => {
            moment.locale('ru');
            date.value = moment().format();
            
            countLastMonthDays();
        })


        function toPrevMonth() {
            date.value = moment(date.value).subtract(1, 'month');
            countLastMonthDays();
        }

        function toNextMonth() {
           date.value = moment(date.value).add(1, 'month');
            countLastMonthDays();
        }

        const remainingDaysOfPrevMonth = ref([]);

        function countLastMonthDays() {
            remainingDaysOfPrevMonth.value = [];

            let i = prevMonthDays.value - (firstWeekdayOfCurrentMonth.value - 2); 
            for(let k = firstWeekdayOfCurrentMonth.value - 1; k > 0; k--){
                remainingDaysOfPrevMonth.value.push(i);
                i++;
            }
        }
        
        return {
            showYear,
            moment,
            date,
            month, 
            year,
            firstWeekdayOfCurrentMonth,
            lastWeekdayOfCurrentMonth,
            prevMonth,
            nextMonth,
            prevMonthDays,
            nextMonthDays,
            countLastMonthDays,
            remainingDaysOfPrevMonth,

            toPrevMonth,
            toNextMonth,
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

    .weekday .short{
        display: none;
    }

    .weekday .long{
        display: block;
    }

    @media screen and (max-width: 1000px){
        .weekday{
            font-size: 13px;
        }
    }   

    @media screen and (max-width: 885px){
        .weekday{
            font-size: 16px;
        }

        .weekday .short{
            display: block;
        }

        .weekday .long{
            display: none;
        }
    }

    .year{
        margin-left: .5em
    }
</style>