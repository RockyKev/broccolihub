<template>
  <div class="timer-wrapper">
    <div class="timer bg-white paper-border">
      <h2 class="timer-header-text">broccoli Day Countdown:</h2>

      <div class="timer-container">
        <div class="timer-bit">
          <span class="timer-count">{{ remainingDays }}</span>
          <span class="timer-text">Days</span>
        </div>

        <div class="timer-line"></div>

        <div class="timer-bit">
          <span class="timer-count">{{ remainingHours }}</span>
          <span class="timer-text">Hours</span>
        </div>

        <div class="timer-line"></div>

        <div class="timer-bit">
          <span class="timer-count">{{ remainingMinutes }}</span>
          <span class="timer-text">Minutes</span>
        </div>

        <div class="timer-line"></div>

        <div class="timer-bit">
          <span class="timer-count">{{ remainingSeconds }}</span>
          <span class="timer-text">Seconds</span>
        </div>
      </div>
    </div>
    <!-- timer end-->
  </div>
</template>


<script>
import { format, differenceInSeconds, addSeconds, compareAsc } from 'date-fns'
import { zonedTimeToUtc } from 'date-fns-tz'

export default {
  name: 'TheHeader',
  data() {
    return {
      remainingDays: '',
      remainingHours: '',
      remainingMinutes: '',
      remainingSeconds: '',
      currentTime: '',
      endTime: '',
      remainingTime: '',
      updateTimeFunc: null,
      addTime: 1
    }
  },
  mounted: function() {
    // set to proper timezone
    // https://date-fns.org/v2.16.0/docs/Time-Zones
    const currentTime = new Date()
    // const currentTime = new Date(2020, 8, 26, 8, 59, 55)
    const endTime = new Date(2021, 4, 4, 4, 0, 0)
    const killTime = new Date(2020, 8, 26, 10, 30, 0)
    const timeZone = 'Europe/Berlin'

    // this.currentTime
    this.currentTime = zonedTimeToUtc(currentTime, timeZone)
    this.endTime = zonedTimeToUtc(endTime, timeZone)
  },
  created() {
    this.updateEverySecond()
  },
  beforeDestroy() {
    clearInterval(this.updateTimeFunc)
  },
  computed: {},
  methods: {
    openModalClick(value) {
      console.log('open modal', value)
      this.$modal.show('modal-agegate-' + value)
    },
    secondsToTime: function(seconds, timeType) {
      let resultArray = []

      switch (timeType) {
        case 'day':
          resultArray[0] = Math.floor(seconds / 86400)
            .toString()
            .padStart(2, '0')

          resultArray[1] = Math.floor(seconds % 86400)
            .toString()
            .padStart(2, '0')
          break

        case 'hour':
          resultArray[0] = Math.floor(seconds / 3600)
            .toString()
            .padStart(2, '0')

          resultArray[1] = Math.floor(seconds % 3600)
            .toString()
            .padStart(2, '0')
          break

        case 'minute':
          resultArray[0] = Math.floor(seconds / 60)
            .toString()
            .padStart(2, '0')

          resultArray[1] = Math.floor(seconds % 60)
            .toString()
            .padStart(2, '0')
          break

        case 'second':
          resultArray[0] = Math.floor(seconds % 60)
            .toString()
            .padStart(2, '0')
          break
      }

      return resultArray
    },
    calculateRemainingTime: function() {
      let remainingDays = this.secondsToTime(this.remainingTime, 'day')
      let remainingHours = this.secondsToTime(remainingDays[1], 'hour')
      let remainingMinutes = this.secondsToTime(remainingHours[1], 'minute')
      let remainingSeconds = this.secondsToTime(remainingMinutes[1], 'second')

      if (this.remainingTime < -5400) {
        // if it's KILL TIME
        console.log('Switch to image')

        // make everything zero
        this.remainingDays = '00'
        this.remainingHours = '00'
        this.remainingMinutes = '00'
        this.remainingSeconds = '00'

        // kill the time
        clearInterval(this.updateTimeFunc)

        // emit and switch the video
        this.switchToStaticImage()
      } else if (this.remainingTime < 0) {
        // if it's LIVE TIME
        console.log('WOOP WOOP WOOP')

        // make everything zero
        this.remainingDays = '00'
        this.remainingHours = '00'
        this.remainingMinutes = '00'
        this.remainingSeconds = '00'

        // emit and switch the video
        this.switchToLivestream()
      } else {
        this.remainingDays = remainingDays[0]
        this.remainingHours = remainingHours[0]
        this.remainingMinutes = remainingMinutes[0]
        this.remainingSeconds = remainingSeconds[0]
      }

      // console.log('remainingDays', remainingDays)
      // console.log('remainingHours', remainingHours)
      // console.log('remainingMinutes', remainingMinutes)
      // console.log('remainingSeconds', remainingSeconds)

      // this.remainingDays = remainingDays[0]
      // this.remainingHours = remainingHours[0]
      // this.remainingMinutes = remainingMinutes[0]
      // this.remainingSeconds = remainingSeconds[0]
    },
    updateEverySecond: function() {
      // https://renatello.com/vue-js-polling-using-setinterval/

      this.updateTimeFunc = setInterval(() => {
        this.currentTime = addSeconds(this.currentTime, this.addTime)
        this.remainingTime = differenceInSeconds(this.endTime, this.currentTime)

        this.calculateRemainingTime()
      }, 1000)
    },
    switchToLivestream: function() {
      // switch the video
      this.$emit('set-livestream')
    },
    switchToStaticImage: function() {
      // switch the video
      this.$emit('set-static')
    },
    debugSubtractTime: function(count) {
      this.addTime = count
    }
  }
}
</script>


<style scoped>
.timer-text-header {
  @apply uppercase text-center text-2xl pb-2;

  @screen lg {
    @apply text-6xl;
    margin: 0;
    padding: 0;
    line-height: 2.5rem;
  }
}

.logo {
  /* flex-grow: 1.5;
  flex-shrink: 1;
  flex-basis: 0; */
  max-width: 30%;

  @screen md {
    max-width: 40%;
  }

  @screen lg {
    flex-grow: unset;
    flex-shrink: unset;
    flex-basis: unset;
    max-width: 20%;
  }
}

.logo img {
  @screen md {
    width: 100%;
  }
}

.timer-wrapper {
  background-color: white;
  border: 0.5rem solid white;
  border-radius: 1rem;
  max-width: 65%;
  @apply mx-auto;

  @screen md {
    max-width: 55%;

    background-color: transparent;
    border: 0.75rem solid white;
    border-radius: 2.5rem;
    flex-grow: 6;
    flex-shrink: 0;
    flex-basis: 0px;
  }

  @screen lg {
    max-width: 50%;
  }

  @screen xl {
    max-width: 55%;
  }
}

.timer-container {
  @apply flex;

  @screen md {
    @apply py-2 w-full;
  }

  @screen lg {
    @apply justify-between;
  }
}

.timer-header-text {
  @screen md {
    @apply text-4xl;
  }

  @screen xl {
    @apply text-5xl;
  }
}

.timer-bit {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  /* border-right: 1px solid black; */
  font-family: 'Insaniburger';
  width: 20%;

  @screen md {
    @apply mx-4;
  }
}

.timer-line {
  width: 1px;
  background-color: green;
  margin: 6px 6px;

  @screen md {
    /* width: 0;
    background-color: green; */
    border-right: 3px solid green;
    margin: 8px 0px;
  }
}

.timer-count {
  line-height: 1.5rem;
  @apply text-2xl px-3;
  /* padding-left: 0.5rem;
  padding-right: 0.5rem; */
}

.timer-text {
  color: green;
  @apply text-xs;

  @screen md {
    @apply text-lg;
  }
}

.timer {
  justify-content: center;
  align-items: center;
  display: flex;
  flex-direction: column;
  /* height: 100%; */

  @screen xl {
    @apply py-4;
  }
}

.timer-count {
  @screen lg {
    @apply text-4xl;
  }
}

.girls-wrapper {
  max-height: 140px;
  @apply overflow-hidden relative;

  @screen md {
    max-height: 240px;
  }

  @screen lg {
    max-height: 360px;
  }
}

.girls {
  @apply flex justify-around relative;

  @screen xl {
    @apply justify-center;
  }
}

.girls img {
  /* max-width: 22%; */
  /* max-height: 190px; */
  /* width: 80px; */
  /* width: 22%; */
  height: 190px;

  @screen md {
    @apply object-contain;
    width: 160px;
    height: 380px;
  }

  @screen lg {
    width: 220px;
    height: 450px;
  }
}

.girl-2 {
  @screen md {
    @apply mr-8;
  }

  @screen xl {
    @apply mr-20 ml-8;
  }
}

.girl-3 {
  @screen xl {
    @apply ml-20 mr-8;
  }
}

.header-cta {
  /* border-radius: 0.3rem; */

  border-radius: 1rem;
  margin: 0 0 0 auto;
  margin-right: 1rem;
  margin-top: 4px;
  padding: 3px 10px;
  width: 100%;

  @apply bg-white;

  @screen md {
    width: 100%;
    margin-left: 2rem;

    border-radius: 2rem;
    margin-right: 3rem;
  }

  @screen lg {
    @apply bg-white flex flex-col justify-center items-center m-0 py-4;
    border-radius: 1.5rem;
    max-width: 22%;
    /* height: 160px; */
  }

  @screen xl {
    @apply py-2;
    max-width: 30%;
  }
}

.header-cta-inner {
  @apply flex justify-around items-center;

  @screen md {
    /* @apply px-8; */
  }

  @screen lg {
    @apply text-center flex-col;
  }

  @screen xl {
    @apply mx-4 my-0;
  }
}
.header-cta-inner-mobile {
  /* @apply flex justify-between items-center py-1; */
  @apply w-full pb-1;

  @screen lg {
    @apply hidden;
  }
}

/* .header-cta-inner-mobile > * {
  margin: 0 4px;

  @screen md {
    @apply mr-6;
  }
} */

.header-cta-inner-lg {
  @apply hidden;

  @screen lg {
    @apply inline;
    width: 180px;
  }

  @screen xl {
    @apply hidden;
  }
}
.header-cta-inner h3 {
  @apply text-center  pr-1;

  @screen md {
    @apply tracking-tighter leading-tight text-3xl pr-0;
  }

  @screen lg {
    @apply text-2xl;
  }

  @screen xl {
    @apply text-4xl;
  }
}

.cta-button {
  /* padding: 8px 20px; */
  @apply text-sm px-5;
  font-family: 'Insaniburger';
  color: white;
  background-color: #d53192;
  border: 2px solid #d53192;
  border-radius: 20px;

  @screen md {
    @apply text-2xl  py-2 px-8;
    border-radius: 2rem;
  }

  @screen lg {
    @apply text-sm tracking-tighter leading-tight mt-1;
    border: 4px solid #d53192;
    width: 100%;
    padding: 8px 8px;
    border-radius: 20px;
  }

  @screen xl {
    border-radius: 40px;
    @apply text-2xl px-12 py-2 my-1;
  }
}

/* .cta-button:last-of-type {
  @screen xl {
    @apply mt-3;
  }
} */

.cta-button:hover {
  @screen lg {
    /* border: 8px solid green;
    padding: 4px 4px; */
    background-color: green;
  }
}
</style>
