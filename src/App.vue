<template>
  <div id="app">
    <Header></Header>
    <Addition></Addition>
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
          priority: "",
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
