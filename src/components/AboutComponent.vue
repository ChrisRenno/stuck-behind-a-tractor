<template>
  <div class="underlay" @click="$emit('showInfo')">
    <div class="about">
      <h3 class="about-heading">About</h3>
      <div class="about-body">
        <template v-for="(faq, index) in faqs" :key="index">
          <p class="about-label">{{faq.label}}</p>
          <p class="about-answer">{{faq.value}}</p>
        </template>
      </div>
    </div>
  </div>

</template>
<script>
export default {
  name: 'AboutComponent',
  data: () => ({
    faqs: [
      {label: `Why isn't there a metric option?`, value: `British testers are the worst.`},
      {label: `I had to stop for some cattle crossing the road, how much time did this cost me?`, value: `Look at your garmin.`},
      {label: `This isn't accurate it doesn't account for fluctuations in speed.`, value: `partridge.gif`},
      {label: `Stop taking the piss, I did actually get held up.`, value: `Calm down, it's just a bit of fun to play around with vue.js.`},
      {label: `Do you even race bro?`, value: `Yeah but the last couple of seasons have been a mess so this is how I get my fun now.`}
    ]
  })
}
</script>
<style lang="css" scoped>
.underlay {
  background-color: rgba(0,0,0,0.6);
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 1;
  backdrop-filter: blur(2px);
}
.about {
  border-radius: 4px;
  overflow: hidden;
  color: #000;
  background: #fff;
  z-index: 2;
  position: sticky;
  top: 16px;
  margin: 0 16px;
}

@media screen and (min-width: 560px){
  .about {
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    position: absolute;
  }    
}

.about-heading {
  color: #f7f6f3;
  background-color: #fc5200;
  padding: 16px;
  margin: 0 0 2px 0;
}
.about-body {
}
.about-label {
  color: #f7f6f3;
  background-color: #fc5200;
  margin: 0;
  padding: 8px 16px;
  font-weight: 700;
}
.about-answer {
  margin: 0;
  padding: 8px 16px;
  font-weight: 700;
  background: #f7f6f3;
}

</style>