<template>

  <section class="button-group">
    <p>Not as much delay as you wanted? Add an excuse...</p>
    <template v-for="excuse in excuses" :key="excuse.value">
      <button  class="add-excuse"  @click="addExcuse(excuse.value)" :disabled="lies.includes(excuse.value)">{{excuse.name}}</button>
    </template>
  </section> 
</template>
<script>
export default {
  name: 'MoreExcuses',
  emits: ['moreLies'],
  // props: {
  //   lies: {
  //     type: Array
  //   }
  // },

  data: () => ({
    excuses: [
      {name: 'Slow Puncture', value: 'Puncture'},
      {name: 'Brake Rub', value: 'Brake'},
      {name: 'Seatpost Slipped', value: 'Seatpost'},
      {name: 'Wrong Aero Socks', value: 'Socks'},
      {name: 'Delayed at Junction', value: 'Junction'},
      {name: 'Covid', value: 'Covid'}
    ],
    lies: []
  }),

  methods: {
    addExcuse(excuse) {
      switch(excuse) {
        case 'Puncture':
          this.$emit('moreLies', ' I noticed I had slow puncture on my way to the start anyway');
          this.lies.push(excuse);
        break;
        case 'Brake':
          this.$emit('moreLies', ' my brakes were definitely dragging a bit');
          this.lies.push(excuse);
        break;
        case 'Seatpost':
          this.$emit('moreLies', ' my seatpost slipped');
          this.lies.push(excuse);
        break;
        case 'Socks':
          this.$emit('moreLies', ' I\'d not brought the right aero socks for the course');
          this.lies.push(excuse);
        break;
        case 'Junction':
          this.$emit('moreLies', ' I had to put my foot down at a junction');
          this.lies.push(excuse);
        break;
        case 'Covid':
          this.$emit('moreLies', ' I\'m pretty sure I\'ve got long COVID too');
          this.lies.push(excuse);
        break;
      }
    }
  }
}
</script>
<style lang="css" scoped>
.add-excuse {
  color: #fff;
  background-color: #fc5200;
  margin: 8px 8px;
  font-size: 18px;
  font-weight: 700;
  border: 2px solid #fc5200;
  padding: 0 16px;
  height: 48px;
  text-transform: uppercase;
  transition: all ease-in-out 0.15s
}
.add-excuse:hover {
  cursor: pointer;
  transform: scale(1.05);
  animation: ease-in-out 0.15s;
}
.add-excuse:disabled {
  background-color: lightgrey;
  color: grey;
  border: 2px solid lightgrey;
  font-weight: 400;
}
</style>