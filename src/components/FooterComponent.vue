<template>

<footer>
  <p class="link" @click="toggleInfo()">About</p>
</footer>

</template>
<script>
export default {
  name: 'FooterComponent',
  methods: {
    toggleInfo() {
      this.$emit('showInfo')
    }
  }
}
</script>
<style lang="css" scoped>

footer {
  color: #f7f6f3;
  background-color: #fc5200;
  position: absolute;
  width: 100%;
  bottom: 0;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  height: 64px;
}

.link {
  font-weight: 700;
}
.link:hover {
  cursor: pointer;
  text-decoration: underline;
}

</style>