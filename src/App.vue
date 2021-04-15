<template>
  <div id="app">
    <div class="add-button">
      <button v-on:click="showAdditionalView"><img src="./assets/img/add.png" alt=""></button>
    </div>
    <div class="items">
      <ul class="tabMenu">
        <li @click="isSelect('toDoList')" :class="{toDoBtn: isSelectToDoBtn}">To Do</li>
        <li @click="isSelect('isDoneList')" :class="{isDoneBtn: isSelectIsDoneBtn}">Done</li>
      </ul>
      <div v-if="isActive === 'toDoList'" :class="{toDoList: isShowToDoList}">
        <ul v-if="items.length">
          <li v-for="(item, index) in narrowDownIncompleteItem" v-bind:key="index">
            <div v-on:click="item.isDone = !item.isDone" class="isDone">✅</div>
            <span class="project-title">
              {{ item.projectTitle }}
            </span>
            <span class="project-task">
              {{ item.title }}
            </span>
            <span class="project-deadline">
              {{ item.deadLine }}
            </span>
            <div v-on:click="deleteItem(index)" class="delete">🗑</div>
          </li>
        </ul>
      </div>

      <div v-else-if="isActive === 'isDoneList'" :class="{isDoneList: isShowIsDoneList}">
        <ul v-if="items.length">
          <li v-for="(item, index) in narrowDownCompleteItem" v-bind:key="index">
            <div v-on:click="item.isDone = !item.isDone" class="isDone">✅</div>
            <span class="project-title">
              {{ item.projectTitle }}
            </span>
            <span class="project-task">
              {{ item.title }}
            </span>
            <span class="project-deadline">
              {{ item.deadLine }}
            </span>
            <div v-on:click="deleteItem(index)" class="delete">🗑</div>
          </li>
        </ul>
      </div>
      <!-- モーダルで表示 -->
      <div class="add-area">
        <transition name="fade">
          <Addition v-if="isAdditionalViewShow"></Addition>
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
        isAdditionalViewShow: false,
        isActive: 'toDoList',
        isDone: false,
        isShowToDoList: true,
        isShowIsDoneList: true,
        isSelectToDoBtn: true,
        isSelectIsDoneBtn: true
      }
    },
    components: {
      Header,
      Addition
      // インポートの時に設定した名前
    },
    methods: {
      updateIsCompleteTodo(todo) {
        todo.isDone = !todo.isDone;
      },
      isSelect: function (index) {
        this.isActive = index;
      },
      showAdditionalView() {
        this.isAdditionalViewShow = true
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
        // dataが変わったときにLSに保存する処理を書く
        this.saveItemsToLocalstorage();
      },
      deep: true
    },
    mounted() {
      this.items = JSON.parse(localStorage.getItem('items')) || [];
    },
    computed: {
      // remaining: function () {
      //   return this.items.filter(function (items) {
      //     return !items.isDone;
      //   });
      // },
      narrowDownIncompleteItem() {
        console.log(this.items)
        console.log("並び替え")
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

  html,
  body {
    height: 100%;
  }

  body {
    background-color: #f9f4ff;
  }

  .tabMenu li {
    background-color: #b980ff;
    padding: 10px;
    border-radius: 30px;
    margin: 8px;
    width: 100px;
    margin: 20px 0 0 20px;
    font-weight: bold;
    color: white;
    cursor: pointer;
  }

  .tabMenu li:hover {
    background-color: white;
    padding: 9px;
    border-radius: 30px;
    border: 1px solid;
    border-color: #b980ff;
    margin: 8px;
    width: 100px;
    margin: 20px 0 0 20px;
    font-weight: bold;
    color: #b980ff;
  }

  .add-button button{
    position: fixed;
    right: 50px;
    bottom: 50px;
    background-color: #b980ff;
    padding: 18px;
    border-radius: 50%;
  }

  .add-button:hover {
    opacity: 0.5;
  }

  .add-button img {
    width: 30px;
  }
  
  .isDone {
    cursor: pointer;
    width: 20px;
  }

  .delete{
    cursor: pointer;
    width: 20px;
  }

  .toDoList li {
    width: 450px;
    height: 100px;
    background-color: white;
    text-align: center;
    padding: 30px;
    border-radius: 20px;
    list-style: none;
    margin: 20px auto;
  }

  .isDoneList li {
    width: 450px;
    height: 100px;
    background-color: #f9f4ff;
    text-align: center;
    padding: 30px;
    border-radius: 20px;
    list-style: none;
    margin: 20px auto;
  }

  .items span {
    display: wrap;
  }

  .taskOptions {
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 10px;
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

  .items-part {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: white;
    -webkit-box-shadow: 0 10px 6px -6px #777;
    -moz-box-shadow: 0 10px 6px -6px #777;
    box-shadow: 0 10px 6px -6px #777;
    width: 300px;
    height: 350px;
    padding: 50px;
    border-radius: 20px;
    color: white;
  }

  .items-part input,
  select {
    border: 0;
    padding: 5px;
    font-size: 12px;
    font-family: Arial, sans-serif;
    color: #aaa;
    border: solid 1px #ccc;
    margin: 0 0 10px;
  }

  .items-part input {
    width: 200px;
  }

  .items-part select {
    width: 50px;
  }

  .items-part label {
    display: block;
    color: black;
  }

  .buttons button {
    font-size: 8px;
    background-color: #b980ff;
    padding: 8px;
    border-radius: 15px;
    margin: 10px;
    width: 100%;
  }

  .buttons {
    display: flex;
  }

  .items {
    display: block;
  }

  .project-title {
    font-weight: bold;
  }

  .project-deadline {
    color: gray;
    font-size: 10px;
  }

</style>
