<template>

  <header class="header">

    <h1>Strava Excuse Generator</h1>

  </header>

</template>
<script>
export default {
  name: 'HeaderComponent'
}
</script>
<style lang="css" scoped>
.header {
  margin: 0;
  padding: 8px 32px 16px;
  display: flex;
  flex-direction: column;
  color: #f7f6f3;
  background-color: #fc5200;
  justify-content: center;
}
@media screen and (min-width: 760px) {
  .header {
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }
}

</style>