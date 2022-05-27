<template>
    <div>
        <div id="head">
            <p @click="monthBack">&lt;</p>
            <h1>{{ monthNames[month] + ' ' + year }}</h1>
            <p @click="monthForward">&gt;</p>
        </div>
        <div id="calendar">
            <div v-for="weekday in weekdays" :key="weekday" class="weekday">
                <p>{{ weekday }}</p>
            </div>
            <div v-for="day in firstDay" :key="day" class="placholder">
            </div>
            <div v-for="day in daysInMonth" :key="day" class="day" :class="{'selected': selectedDay == day}" @click="selectDay(day)">
                <p>{{ day.getDate() }}</p>
            </div>
        </div>
        <div id="events">
            <div v-for="event in visibleEvents" :key="event">
                <h2>{{ event.name }}</h2>
                <span>{{ event.date }}</span>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, computed } from 'vue'

let weekdays = ["Montag", "Dienstag", "Mittwoch", "Donnerstag", "Freitag", "Samstag", "Sontag"]
let month = ref(new Date().getMonth());
let monthNames = ["Januar", "Februar", "März", "April", "Mai", "Juni", "Juli", "August", "September", "Oktober", "November", "Dezember"]
let year = ref(new Date().getFullYear());
let selectedDay = ref(null);
let events = {
    1000000: {
        name: 'Ein Event',
        date: '2022-05-15'
    },
    1000001: {
        name: 'Ein zweites Event',
        date: '2022-05-14'
    },
    1000002: {
        name: 'Ein zweites Event am gleichen Tag',
        date: '2022-05-14'
    },
    1000003: {
        name: 'Noch ein Event',
        date: '2022-05-13'
    }
}

let daysInMonth = computed(() => {
    let temp = [];
    let days = new Date(year.value, month.value + 1, 0).getDate();

    for (let i = 1; i <= days; i++) {
        temp.push(new Date(year.value, month.value, i));
    }

    return temp;
});

let firstDay = computed(() => {
    let temp = new Date(year.value, month.value, 1).getDay() - 1;
    temp == -1 ? temp = 6 : null;
    return temp;
});

let visibleEvents = computed(() => {
    let temp = {};

    for (let [key, value] of Object.entries(events)) {
        new Date(value.date).toDateString() == new Date(selectedDay.value).toDateString() ? temp[key] = value : null;
    }

    return temp;
})

function selectDay(day) {
    selectedDay.value = day;
}

function monthBack() {
    month.value -= 1;
    if (month.value == -1) {
        month.value = 11;
        year.value -= 1;
    }
}

function monthForward() {
    month.value += 1;
    if (month.value == 12) {
        month.value = 0;
        year.value += 1;
    }
}
</script>

<style>
#head {
    display: flex;
    justify-content: space-between;
}

#head > p {
    padding: 16px;
    background-color: #eeeeee;
    cursor: pointer;
}

#calendar {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
    grid-template-rows: 40px 1fr 1fr 1fr 1fr 1fr 1fr;
    gap: 10px 10px;
}

.weekday {
    font-weight: bold;
    text-align: center;
}

.day {
    box-sizing: border-box;
    padding: 10px;
    height: 140px;
    background-color: aquamarine;
}

.day:hover {
    background-color:rgb(95, 193, 160);
}

.selected {
    background-color:rgb(95, 193, 160);
}

.placholder {
    background-color: none;
}
</style>
