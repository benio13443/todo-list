<template>
  <div id="app">
    <Header></Header>
    <div class="add-button">
      <button @click="addMethod"><img src="./assets/img/add.png" alt=""></button>
    </div>
    <div class="add-area">
      <Addition v-if="addShow"></Addition>
    </div>
    <ul>
      <!-- <li v-for="(item, index) in items" v-bind:key="index"> -->
      <li v-for="(item, index) in sortedPriorityByValue" v-bind:key="index">
        {{ item.title }}{{ item.deadLine }}
        <div v-on:click="deleteItem(index)" class="delete">delete</div>
      </li>
      <pre> {{ $data }} </pre>
    </ul>
  </div>
</template>
<script>
  import Header from "./components/Header.vue";
  import Addition from "./components/Addition.vue";
  // コンポーネント読み込み
  export default {
    data() {
      return {
        items: [],
        addShow: false,
        priority: this.items.$children.deadLine * this.$children.items.importance
      }
    },
    components: {
      Header,
      Addition
      // インポートの時に設定した名前
    },
    methods: {
      addMethod() {
        this.addShow = true
      },
      deleteItem: function (index) {
        if (confirm('Are you sure?')) {
          this.items.splice(index, 1);
          this.saveItems();
        }
      },
      saveItems() {
        localStorage.setItem('items', JSON.stringify(this.items));
      },
    },
    watch: {
      items: function () {
        // itemsの値が変わったときに動く関数
      }
    },
    mounted() {
      this.items = JSON.parse(localStorage.getItem('items')) || [];
    },
    computed: {
      remaining: function () {
        return this.items.filter(function (items) {
          return !items.isDone;
        });
      },
      sortedPriorityByValue(){
        return this.priority.sort(function(a, b){
          return a.priority - b.prioroty;
        });
      }
    }
  };

</script>
<style>
  /* @import リセットCSS パス */
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
