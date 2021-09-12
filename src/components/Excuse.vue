<template>
  <section class="alert-wrapper" ref="excuse">
    <p class="excuse">"{{excuse}}"</p>
    <p class="logic">{{message}}</p>
  </section>
</template>
<script>
export default {
  name: 'Excuse',
  props: {
    excuse: {
      type: String
    },
    message: {
      type: String
    }
  },
  mounted() {
    this.goto('excuse')
  },
  updated() {
    this.goto('excuse')
  },
  methods: {
    goto(refName) {
      var element = this.$refs[refName];
      // console.log(element);
      var top = element.offsetTop;
      window.scrollTo(0, top);
    }
  }
}
</script>
<style lang="css" scoped>
.alert-wrapper {
  display: flex;
  flex-direction: column;
}
.excuse {
  background-color: #fc5200;
  color: #fff;
  font-weight: 700;
  font-size: 18px;
  padding: 8px 16px;
  border-radius: 4px;
  margin: 0 0 8px 0;
}
.logic {
  background-color: #000;
  color: #fff;
  font-weight: 300;
  font-size: 18px;
  padding: 8px 16px;
  border-radius: 4px;
  margin: 0 0 8px 0;
}

</style>