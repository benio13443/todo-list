<template>
  <div id="app">
    <Header></Header>
    <Home></Home>
    <div class="add-button">
      <button @click="addMethod"><img src="./assets/img/add.png" alt=""></button>
    </div>
    <div class="add-area">
      <Addition v-if="addShow"></Addition>
    </div>
  </div>
</template>
<script>
  import Header from "./components/Header.vue";
  import Addition from "./components/Addition.vue";
  import Home from "./components/Home.vue";
  // コンポーネント読み込み
  export default {
    data() {
      return {
        addShow: false
      }
    },
    components: {
      Header,
      Addition,
      Home
      // インポートの時に設定した名前
    },
    methods: {
      addMethod() {
        this.addShow = !this.addShow
      }
    },
    watch: {
      items: function () {
        // itemsの値が変わったときに動く関数
      }
    },
    mounted: function () {
      this.items = JSON.parse(localStorage.getItem('items')) || [];
    },
    computed: {
      remaining: function () {
        return this.items.filter(function (items) {
          return !items.isDone;
        });
      }
    },
  };

</script>
<style>
  .add-button {
    position: fixed;
    right: 50px;
    bottom: 50px;
    transition: 1s;
    opacity: 0.7;
  }

  .add-button img {
    width: 30px;
    background-color: aquamarine;
    border: none;
    /* 枠線を消す */
    outline: none;
    /* クリックしたときに表示される枠線を消す */
    background: transparent;
  }

</style>
