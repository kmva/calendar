<template>
  <div class="event" :class="event.type">
        {{ moment(event.date).format('h:mm') }}
        {{ event.title }}
  </div>
</template>

<script>
import moment from 'moment'

export default {
    props: {
        event: {
            type: Object,
            required: true,
        }
    },

    setup() {
         return {
            moment,
        }
    }
}
</script>

<style>
    .event{
        text-align: left;
        border-radius: 20px;
        padding: .3em .7em;
        font-size: 13px;
        overflow: hidden; 
        white-space: nowrap;
        text-overflow: ellipsis;
        max-width: 100%;
        display: inline-block;
        transition: .3s;
    }

    .event:hover, .event:active{
        overflow: auto;
        text-overflow: initial;
        position: relative;
        z-index: 1000;
        max-width: initial;
    }

    .red{
        background-color: rgba(255, 0, 0, .3);
        color: #B30000;
    }

    .orange{
        background-color: rgba(255, 136, 0, 0.3);
        color: #b36e00;
    }

    .green{
        background-color: rgba(0, 255, 0, .3);
        color: #005a05;
    }
</style>