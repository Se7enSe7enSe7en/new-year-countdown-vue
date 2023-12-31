<script setup>
import CountdownHeader from "@/components/CountdownHeader.vue";
import CountdownSegment from "@/components/CountdownSegment.vue";
import { onMounted, ref, computed, watch, onUnmounted } from "vue";

const now = ref(new Date());
const timer = setInterval(() => (now.value = new Date()), 1000);
onUnmounted(() => clearInterval(timer));

const newYearFromNow = computed(() => {
  const newDate = new Date(now.value);
  newDate.setFullYear(now.value.getFullYear() + 1);
  newDate.setMonth(0, 1);
  newDate.setHours(0, 0, 0, 0);
  // console.log("setDateToNewYear: ", newDate);
  return newDate;
});

const millisecondsFromNewYear = computed(
  () => newYearFromNow.value.valueOf() - now.value.valueOf()
);

// conversion: 1 day to milliseconds

// (24 hrs / 1 day ) x (60 min / 1 hr) x (60 sec / 1 min) x (1000 millisec. / 1 sec.)
const dayInMilliseconds = 24 * 60 * 60 * 1000;
const hourInMilliseconds = 60 * 60 * 1000;
const minuteInMilliseconds = 60 * 1000;
const secondInMilliseconds = 1000;

// conversion: milliseconds from new year to days
const daysBeforeNewYear = computed(() => {
  return Math.floor(millisecondsFromNewYear.value / dayInMilliseconds);
});

// const nextDay = computed(
//   () =>
//     new Date(
//       new Date(new Date().setDate(now.value.getDate() + 1)).setHours(0, 0, 0, 0)
//     )
// );
// const timeBeforeNextDay = computed(() => new Date(nextDay.value - now.value));

// hour
const hoursBeforeNextDayInMilliseconds = computed(
  () => millisecondsFromNewYear.value % dayInMilliseconds
);
const hoursBeforeNextDay = computed(() =>
  Math.floor(hoursBeforeNextDayInMilliseconds.value / hourInMilliseconds)
);
// const hoursBeforeNextDay = computed(() => timeBeforeNextDay.value.getHours());

// minute
const minutesBeforeNextHourInMilliseconds = computed(
  () => millisecondsFromNewYear.value % hourInMilliseconds
);
const minutesBeforeNextHour = computed(() =>
  Math.floor(minutesBeforeNextHourInMilliseconds.value / minuteInMilliseconds)
);
// const minutesBeforeNextHour = computed(() =>
//   timeBeforeNextDay.value.getMinutes()
// );

// second
const secondsBeforeNextMinuteInMilliseconds = computed(
  () => millisecondsFromNewYear.value % minuteInMilliseconds
);
const secondsBeforeNextMinute = computed(() =>
  Math.floor(secondsBeforeNextMinuteInMilliseconds.value / secondInMilliseconds)
);
// const secondsBeforeNextMinute = computed(() =>
//   timeBeforeNextDay.value.getSeconds()
// );
</script>
<template>
  <div class="app-wrapper">
    <div class="countdown-box">
      <CountdownHeader />
      <main class="flex justify-center">
        <CountdownSegment
          data-test="days"
          label="days"
          :number="daysBeforeNewYear"
        />
        <CountdownSegment
          data-test="hours"
          label="hours"
          :number="hoursBeforeNextDay"
        />
        <CountdownSegment
          data-test="minutes"
          label="minutes"
          :number="minutesBeforeNextHour"
        />
        <CountdownSegment
          data-test="seconds"
          label="seconds"
          :number="secondsBeforeNextMinute"
        />
      </main>
    </div>
  </div>
</template>

<style scoped>
.app-wrapper {
  @apply flex items-center justify-center w-full h-full p-10;
}
.countdown-box {
  @apply shadow-md relative bg-white p-5 rounded-lg border-gray-100 border-[1px];
}
body {
  @apply bg-gray-100;
}
</style>
