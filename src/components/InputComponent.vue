<template>
  <section class="input-section" ref="form">
    <form v-show="!excuse">
    <div class="field-group">
      <p class="label">What did you get stuck behind?</p>
      <!-- <select id="options" name="options" v-model="selected" @change="updateOption(selected)"> -->
      <select id="options" name="options" v-model="selected">
        <option v-for="(option, index) in options" :value="option" :key="index">{{ option }}</option>
      </select>
    </div>
    <div class="field-group">
      <p class="label">What was your finish time?</p>
      <!-- <input type="time" ref="speed" min="00:00:00" v-model="time" step="2" :disabled="selected === 'offending obstruction'" /> -->
      <div class="time-wrapper">

        <select class="inline-input" v-model="riderHours" :disabled="selected === 'offending obstruction'" @change="setTime()">
          <option v-for="(hour, index) in hourPicker" :key="index">{{hour}}</option>
        </select>

        <select class="inline-input" v-model="riderMins" :disabled="selected === 'offending obstruction'" @change="setTime()">
          <option v-for="(minute, index) in minSecPicker" :key="index">{{minute}}</option>
        </select>

        <select class="inline-input" v-model="riderSecs" :disabled="selected === 'offending obstruction'" @change="setTime()">
          <option v-for="(second, index) in minSecPicker" :key="index">{{second}}</option>
        </select>
      </div>
    </div>
    <div class="field-group">
      <p class="label">How fast were you moving before you got delayed?</p>
      <input type="number" ref="speed" v-model="speed" :disabled="!time"/>mph
    </div>
    <div class="field-group">
      <p class="label">How fast was the {{selected}} moving?</p>
      <input type="number" ref="tractor" v-model="tractorSpeed" :disabled="!speed" />mph
    </div>
    <p class="alert" v-if="parseFloat(tractorSpeed) > parseFloat(speed)">Drafting.  You should be ashamed.</p>
    <div class="field-group">
      <p class="label">What distance were you stuck for?</p>
      <input type="number" ref="distance" v-model="distance" :disabled="!tractorSpeed || tractorSpeed > speed" />miles
    </div>
    </form>
    <div class="field-group">
      <button class="submit" v-if="!excuse" @click="calculate()" :disabled="!distance || excuse">Generate Excuse</button>
      <button v-if="excuse" class="reload" @click="reset()">Reload</button>
    </div>
  </section>
</template>
<script>
export default {
  name: 'InputComponent',

  data: () => ({
    options: [
    'Tractor', 'Potato Lorry', 'Learner Driver', 'Milk Float', 'Funeral Cortege', 'Old Lady', 'Steam Engine'
    ],
    selected: 'offending obstruction',
    hourPicker: ['00','01','02','03','04','05','06','07','08','09','10','11','12','13','14','15','16','17','18','19','20','21','22','23','24'],
    minSecPicker: ['00','01','02','03','04','05','06','07','08','09','10','11','12','13','14','15','16','17','18','19','20','21','22','23','24','25','26','27','28','29','30','31','32','33','34','35','36','37','38','39','40','41','42','43','44','45','46','47','48','49','50','51','52','53','54','55','56','57','58','59'],
    time: null,
    riderHours: '00',
    riderMins: '00',
    riderSecs: '00',
    speed: null,
    tractorSpeed: null,
    distance: null,
    excuse: null,
    message: null,
  }),
 methods: {
  calculate() {
    var target = (this.distance / this.speed);
    var actual = (this.distance / this.tractorSpeed);
    var diff = (actual - target);
    var onfer = this.timeStringToNumber(this.time) - diff;
    // console.log('time', this.time,'time to number',this.timeStringToNumber(this.time), 'diff', diff, 'timesetterdiff',this.timeSetter(diff), 'onfer', onfer, 'onfer to time', this.timeSetter(onfer) )
    this.excuse = `I was on for a ${this.timeSetter(onfer)}, until I got held up by a ${this.selected}.`
    this.message = `If you'd kept going at your pre-delayed speed you would have covered the ${this.distance}miles in ${this.timeSetter(target)}.  You actually covered it in ${this.timeSetter(actual)}. You were delayed for an additional ${this.timeSetter(diff)}.`;
    this.$emit('updatedExcuse', this.excuse);
    this.$emit('updatedMessage', this.message);
   },
   timeSetter(time) {
      var decimalTimeString = time;
      var decimalTime = parseFloat(decimalTimeString);
      decimalTime = decimalTime * 60 * 60;
      // decimalTime = decimalTime * 60
      var hours = Math.floor((decimalTime / (60 * 60)));
      decimalTime = decimalTime - (hours * 60 * 60);
      var minutes = Math.floor((decimalTime / 60));
      decimalTime = decimalTime - (minutes * 60);
      var seconds = Math.round(decimalTime);
      if(hours < 10)
      {
        hours = "0" + hours;
      }
      if(minutes < 10)
      {
        minutes = "0" + minutes;
      }
      if(seconds < 10)
      {
        seconds = "0" + seconds;
      }
      return("" + hours + ":" + minutes + ":" + seconds);
   },
  timeStringToNumber( time ) {
    return ((new Date(("01 Jan 1970 " + time || "").replace(".",":") + " GMT")) / 3600000) || 0;
  },
  updateOption(value)   {
    this.$emit('optionChanged', value)
  },
  reset() {
    this.selected = null
    this.time = null
    this.riderHours = '00'
    this.riderMins = '00'
    this.riderSecs = '00'
    this.speed = null
    this.tractorSpeed = null
    this.distance = null
    this.excuse = null
    this.message = null
    this.$emit('reset')
    // this.goto('form')
  },
  setTime() {
    // console.warn('setting time', this.riderHours, this.riderMins, this.riderSecs, 'time', this.time)
    this.time = this.riderHours + ':' + this.riderMins + ':' + this.riderSecs;

    // console.warn('setting time', this.riderHours, this.riderMins, this.riderSecs, 'time', this.time)
  },
 }
}
</script>
<style lang="css" scoped>
/* .input-section { */
  /* border: 2px solid #0d3b66;
  border-radius: 4px; */
  /* background: #f6f9f1; */
  /* margin: 0 8px; */
  /* max-width: 380px; */
/* } */

/* @media screen and (min-width: 432px) {
  .input-section {
    border: 2px solid #0d3b66;
    border-radius: 4px;
    margin: 0 auto;
    width: 400px;
  }
}

@media screen and (min-width: 760px) {
  .input-section {
    width: 500px;
  }
} */
.submit {
  color: #E6EED6;
  background-color: #fc5200;
  margin: 8px 0;
  font-size: 18px;
  font-weight: 700;
  border: 2px solid #fc5200;
  padding: 0 16px;
  height: 48px;
  text-transform: uppercase;
  transition: all ease-in-out 0.15s
}
.submit:disabled {
  background-color: lightgrey;
  color: grey;
  border: 2px solid lightgrey;
  font-weight: 400;
}
.submit:hover {
  cursor: pointer;
  transform: scale(1.05);
  animation: ease-in-out 0.15s;
}
input, select, time {
  font-size: 16px;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}

.reload {
  color: #fc5200 ;
  background-color: #000;
  margin: 8px 0 0 8px;
  font-size: 18px;
  font-weight: 700;
  border: 2px solid #fc5200;
  padding: 0 16px;
  height: 48px;
  text-transform: uppercase;
  transition: all ease-in-out 0.15s
}

.reload:hover {
  cursor: pointer;
  transform: scale(1.05);
  animation: ease-in-out 0.15s;
}

.inline-input {
  margin-right: 8px;
}

.alert {
  background-color: #fc5200;
  color: #fff;
  padding: 4px;
  border-radius: 4px;
}

</style>