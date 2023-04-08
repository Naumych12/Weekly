<template>
  <q-page>
    <InfoPage v-if="showInfo" :shifts="shifts" @on-cancel="showInfo = false"></InfoPage>
    <LovePage v-if="showLove" @on-cancel="showLove = false"></LovePage>

    <q-btn @click="showInfo = true" flat style="position: fixed; right: -5px; top: 5px; z-index: 100; color: #aaaaaa" icon="info" size="20px"></q-btn>
    <q-btn @click="showLove = true" flat style="position: fixed; left: -5px; top: 5px; z-index: 100; color: #e5a3a3" icon="favorite" size="20px"></q-btn>

    <div v-for="month in monthAmount" v-bind:key="month" class="month-div">
    <a id="currentDayEl" v-if="ifHasCurrentDate(month)"></a>
    <div class="month-header" v-text="getDayByMonthNumber(month).monthName + ' ' + getDayByMonthNumber(month).year"></div>
    <div class="week-div">
      <div>Пн</div>
      <div>Вт</div>
      <div>Ср</div>
      <div>Чт</div>
      <div>Пт</div>
      <div>Сб</div>
      <div>Нд</div>
    </div>
    <div style="border-radius: 10px">
      <div v-for="week in getWeeksByMonthNumber(month)" v-bind:key="week"
           :style="'background:' + week[0].color">
        <div v-for="day in week" v-bind:key="day" class="day-div"
        :style="day.isCurrentDay ? 'background: #FF8D7B; border-radius: 5px; color: white' : ''">
          {{ day.day }}
        </div>
      </div>
    </div>
  </div>
  </q-page>
</template>

<script setup>
import {ref, onMounted} from 'vue'
import lovePage from "./love-page"
import infoPage from "./info-page"
import InfoPage from "pages/info-page";
import LovePage from "pages/love-page";

const showLove = ref(false)
const showInfo = ref(false)
const startDate = ref(new Date(2023, 2, 27))
const shifts = ref([
  {
    text: "Перша зміна",
    color: "#eef7e1"
  },
  {
    text: "Друга зміна",
    color: "#faf1e6"
  },
  {
    text: "Нічна зміна",
    color: "#ebe3ff"
  }
])
const days = ref([])
const monthAmount = ref(1)
fillDays()

onMounted(() =>{
  setTimeout(() => {
  let el = window.document.getElementById("currentDayEl")
  if(el) {
    el.scrollIntoView({behavior: "smooth", block: "start", inline: "nearest"})
  }
  }, 0)
})

function fillDays() {
  let visibleDays = getDaysDiff(startDate.value, new Date()) + 1366
  let currentDate = startDate.value
  let shiftCount = 0;
  for (let i = 0; i <= visibleDays; i++) {
    shiftCount++
    if(shiftCount > 28) {
      shiftCount = 1
    }
    let shiftNumber = -1
    if(shiftCount >= 1 && shiftCount <= 7) {
      shiftNumber = 2
    } else if(shiftCount >= 8 && shiftCount <= 14
      || shiftCount >= 22 && shiftCount <= 28) {
      shiftNumber = 0
    } else if(shiftCount >= 15 && shiftCount <= 21) {
      shiftNumber = 1
    }

    days.value.push({
      day: currentDate.getDate(),
      month: currentDate.getMonth(),
      year: currentDate.getUTCFullYear(),
      shift: shifts.value[shiftNumber].text,
      color: shifts.value[shiftNumber].color,
      monthName: getNameOfMonth(currentDate.getMonth()),
      numberByMonth: monthAmount.value,
      dayOfWeekNumber: currentDate.getDay() === 0 ? 6 : currentDate.getDay() - 1,
      isCurrentDay: new Date().getDate() === currentDate.getDate()
      && new Date().getMonth() === currentDate.getMonth()
      && new Date().getFullYear() === currentDate.getFullYear()
    })
    currentDate = addDays(currentDate, 1)

    if(i > 0 && days.value[i - 1].monthName !== days.value[i].monthName) {
      monthAmount.value++
      days.value[days.value.length - 1].numberByMonth++
    }

  }
}

function getNameOfMonth(monthNumber) {
  const options = { month: 'long' };
  let date = new Date(Date.UTC(2023, monthNumber))
  let month = date.toLocaleString('UK-UA', options)
  return month.charAt(0).toUpperCase() + month.slice(1);
}

function getDaysDiff(date1, date2) {
  const oneDay = 24 * 60 * 60 * 1000;
  const diffInMilliseconds = Math.abs(date1.getTime() - date2.getTime());
  return Math.floor(diffInMilliseconds / oneDay);
}

function addDays(date, days) {
  const result = new Date(date);
  result.setDate(result.getDate() + days);
  return result;
}

function getWeeksByMonthNumber(number) {
  if(days.value.length === 0) {
    return []
  }
  let month = days.value.filter(x => x.numberByMonth === number)
  let startSpace = []
  let endSpace = []
  for(let i = 1; i <= month[0].dayOfWeekNumber; i++) {
    startSpace.push({
      day: "",
      color: month[0].color
    })
  }
  for(let i = 1; i <= 6 - month[month.length - 1].dayOfWeekNumber; i++) {
    endSpace.push({
      day: "",
      color: month[month.length - 1].color
    })
  }

  month = startSpace.concat(month).concat(endSpace)

  const chunkSize = 7;
  let weeks = []
  for (let i = 0; i < month.length; i += chunkSize) {
    const chunk = month.slice(i, i + chunkSize);
    weeks.push(chunk);
  }

  return weeks
}

function getDayByMonthNumber(number) {
  if(days.value.length === 0) {
    return []
  }
  return days.value.find(x => x.numberByMonth === number)
}

function ifHasCurrentDate(number) {
  let month = days.value.filter(x => x.numberByMonth === number)
  return month.find(x => x.isCurrentDay) !== undefined
}
</script>
