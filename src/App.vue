<template>
  <div id="app">
    <Header></Header>
    <div class="add-button">
      <button v-on:click="addMethod"><img src="./assets/img/add.png" alt=""></button>
    </div>
    <div class="add-area">
      <Addition v-if="addShow"></Addition>
    </div>
    <div class="items">
      <h1>完了済みのタスク</h1>
      <ul v-if="items.length">
        <li v-for="(item, index) in narrowDownCompleteItem" v-bind:key="index">
          <span :class="{ 'isDone':item.isDone }">
            {{ item.title }}
          </span>
          <span>
            {{ item.deadLine }}
          </span>
          <div v-on:click="deleteItem(index)" class="delete">delete</div>
          <input type="checkbox" v-model="item.isDone">
        </li>
      </ul>

      <h1>未完了のタスク</h1>
      <ul v-if="items.length">
        <li v-for="(item, index) in narrowDownIncompleteItem" v-bind:key="index">
          <span>
            {{ item.title }}
          </span>
          <span>
            {{ item.deadLine }}
          </span>
          <div v-on:click="deleteItem(index)" class="delete">delete</div>
          <input type="checkbox" v-model="item.isDone">
        </li>
      </ul>
    </div>
    <pre> {{ $data }} </pre>
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
        addShow: false
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
      narrowDownIncompleteItem(){
        return this.items.filter(function(item){
          return item.isDone == false
        }
        )
      },
      narrowDownCompleteItem(){
        return this.items.filter(function(item){
          return item.isDone == true
        }
        )
      },
      sortedPriorityByValue() {
        return this.items.sort(function (a, b) {
          return a.priority - b.prioroty;
        });
      }
    }
  };

</script>
<style>
  /* @import "https://unpkg.com/ress/dist/ress.min.css"; */

  #app {
    width: 500px;
    text-align: center;
  }

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

  .items ul {
    width: 500px;
    height: 100px;
    text-align: center;
  }

  .items span {
    display: wrap;
  }

  .isDone{
    text-decoration: line-through;
  }

</style>
