<template>
  <div id="app">
    <ul class="tabMenu">
      <li @click="isSelect('1')">In Progress</li>
      <li @click="isSelect('2')">Done</li>
    </ul>
    <h3>My Tasks</h3>
    <div class="add-button">
      <button v-on:click="addMethod"><img src="./assets/img/add.png" alt=""></button>
    </div>
    <div class="items">
      <div v-if="isActive === '1'" :class="{'active': isActive === '1'}">
        <h3>未完了済みのタスク</h3>
        <ul v-if="items.length">
          <li
            v-for="(item, index) in narrowDownIncompleteItem && narrowDownIncompleteItem.slice().sort((a, b) => a.priority - b.priority)"
            v-bind:key="index">
            <span>
              Project Title:{{ item.projectTitle }}
            </span>
            <span>
              Task Title:{{ item.title }}
            </span>
            <span>
              Dead Line:{{ item.deadLine }}
            </span>
            <div class="taskOptions">
              <div v-on:click="deleteItem(index)" class="delete">delete</div><input type="checkbox"
                v-model="item.isDone">
            </div>
          </li>
        </ul>
      </div>

      <div v-else-if="isActive === '2'" :class="{'active': isActive === '2'}">
        <h3>完了済みのタスク</h3>
        <ul v-if="items.length">
          <li
            v-for="(item, index) in narrowDownCompleteItem && narrowDownCompleteItem.slice().sort((a, b) => a.priority - b.priority)"
            v-bind:key="index">
            <span>
              Project Title:{{ item.projectTitle }}
            </span>
            <span>
              Task Title:{{ item.title }}
            </span>
            <span>
              Dead Line:{{ item.deadLine }}
            </span>
            <div class="taskOptions">
              <div v-on:click="deleteItem(index)" class="delete">delete</div><input type="checkbox"
                v-model="item.isDone">
            </div>
          </li>
        </ul>
      </div>
      <!-- モーダルで表示 -->
      <div class="add-area">
        <transition name="fade">
          <Addition v-if="addShow"></Addition>
        </transition>
      </div>
      <!-- <h3>並び替え</h3>
      <ul v-if="items.length">
        <li v-for="(item, index) in items.slice().sort((a, b) => a.priority - b.priority)" v-bind:key="index">
          <span :class="{ 'isDone':item.isDone }">
            {{ item.title }}
          </span>
          <span>
            {{ item.deadLine }}
          </span>
          <div v-on:click="deleteItem(index)" class="delete">delete</div>
          <input type="checkbox" v-model="item.isDone">
        </li>
      </ul> -->
    </div>
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
        isActive: '1'
      }
    },
    components: {
      Header,
      Addition
      // インポートの時に設定した名前
    },
    methods: {
      isSelect: function (num) {
        this.isActive = num;
      },
      addMethod() {
        this.addShow = true
      },
      deleteItem: function (index) {
        this.$swal({
            title: "削除しますか？",
            icon: "warning",
            buttons: true,
            dangerMode: true,
          })
          .then((ok) => {
            if (ok) {
              this.items.splice(index, 1);
              this.saveItemsToLocalstorage();
            }
          });
      },
      saveItemsToLocalstorage() {
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
      narrowDownIncompleteItem() {
        return this.items.filter(function (item) {
          return item.isDone == false
        })
      },
      narrowDownCompleteItem() {
        return this.items.filter(function (item) {
          return item.isDone == true
        })
      },
    }
  };

</script>
<style>
  @import "https://unpkg.com/ress/dist/ress.min.css";

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
  }

  .items ul {
    width: 500px;
    height: 100px;
    text-align: center;
    position: relative;
  }

  .items span {
    display: wrap;
  }

  .isDone {
    text-decoration: line-through;
  }

  .taskOptions {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .tabMenu {
    display: flex;
    list-style: none;
  }

  .fade-enter-active,
  .fade-leave-active {
    will-change: opacity;
    transition: opacity 225ms cubic-bezier(0.4, 0, 0.2, 1) 0ms;
  }

  .fade-enter,
  .fade-leave-to {
    opacity: 0
  }

  .add-area{
    position: fixed;
    background-color: white;
    box-shadow: 2px 2px 4px gray;
    width: 300px;
  }
</style>
