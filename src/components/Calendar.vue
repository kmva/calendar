<template>
<div>
  <h1>
      <button @click="prevMonth">&#8592;</button>
        {{moment(date).format('MMMM')}}
        <button @click="nextMonth">&#8594;</button>
      <span v-if="showYear">{{moment(date).format('YYYY')}}</span>
    </h1>

    <div class="calendar">
        <div class="calendar-wrapper">

        </div>
    </div>
</div>
</template>

<script>
import { computed, ref, onMounted } from 'vue'
import moment from 'moment'

export default {
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
            prevMonth,
            nextMonth,
        }
    }
}
</script>

<style>

</style>