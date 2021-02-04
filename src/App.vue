<template>
  <div id="app">
    <Header></Header>
    <Addition></Addition>
    <!-- <div class="items-part">
      <form v-on:submit.prevent>
        <input type="text" v-model="newItem.title">
        <label>締め切り時間:
          <input type="date" value="00:00" v-model="newItem.deadLine">
        </label>
        <label>見積り時間:
          <input type="time" value="00:00" v-model="newItem.expectedTime">
        </label>
        <button v-on:click="addItem">create</button>
        <!-- value属性値は、初期入力値となる。
        時間入力欄のvalue属性値は、hh:mm:ssの形式で入力する -->
      <!-- </form>
      <ul>
        <li v-for="(item, index) in items" v-bind:key="index">{{ item.title }}{{ item.deadLine }}{{ item.expectedTime }}
          <div v-on:click="deleteItem(index)" class="delete">
          </div>
        </li>
        <pre> {{ $data }} </pre>
      </ul>
    </div> --> -->
    <Home></Home>
    <!-- <div class="add-icon">
      <img src="add_icon.png" class="addIcon">
    </div> -->
  </div>
</template>
<script>
  import Header from "./components/Header.vue";
  import Addition from "./components/Addition.vue";
  import Home from "./components/Home.vue";
  // コンポーネント読み込み
  export default {
    components: {
      Header,
      Addition,
      Home
      // インポートの時に設定した名前
    },
    data() {
      return {
        newItem: {
          title: "",
          deadLine: "",
          expectedTime: "",
          priority:"",
        },
        items: [],
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
    methods: {
      addItem: function () {
        var item = {
          title: this.newItem.title,
          deadLine: this.newItem.deadLine,
          expectedTime: this.newItem.expectedTime,
          priority: this.newItem.priority,
          isDone: false
        };
        this.items.push(item);
        this.setItems();
        this.newItem = '';
      },
      deleteItem: function (index) {
        if (confirm('Are you sure?')) {
          this.items.splice(index, 1);
          this.setItems();
        }
      },
      setItems() {
        localStorage.setItem('items', JSON.stringify(this.items));
      },
    },
    purge: function () {
      if (!confirm('Delete finished?')) {
        return;
      }
      this.items = this.remaining;
    },
    //わかりやすい動詞を使う。目的語を変える。
    computed: {
      remaining: function () {
        return this.items.filter(function (items) {
          return !items.isDone;
        });
      }
    },
  };
</script>
