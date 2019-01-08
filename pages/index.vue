<template>
  <section class="container">
    <div>
      <h1 class="title">
        ネットでじゃんけん
      </h1>
      <div class="links">
        <nuxt-link
          :to="{ path: getRoomURL, query: { userid: userId }}"
          target="_blank"
          class="button--green">部屋を作る</nuxt-link>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  data () {
    return {
      roomId: undefined,
      userId: undefined,
    }
  },
  created: function () {
    var max = 1000;
    this.roomId = Math.floor(Math.random() * Math.floor(max));
    this.userId = Math.floor(Math.random() * Math.floor(max));
  },
  computed: {
    getRoomURL() {
      return "/rooms/" + this.roomId
    }
  }
}
</script>

<style>

.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}


</style>
