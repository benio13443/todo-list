<template>
  <div id="app">
    <div class="add-button">
      <button v-on:click="showAdditionalView"><img src="./assets/img/add.png" alt=""></button>
    </div>
    <div class="items">
      <div class="tabMenu">
        <button @click="switchTabs(true)" :class="{'activeTab': isActiveIncompleteList}">To Do</button>
        <button @click="switchTabs(false)" :class="{'activeTab': !isActiveIncompleteList}">Done</button>
      </div>
      <div v-if="isActiveIncompleteList" :class="{toDoList: isShowToDoList}">
        <ul v-if="items.length">
          <li v-for="(item, index) in narrowDownIncompleteItem" v-bind:key="index">
            <div>
              <button v-on:click="item.isDone = !item.isDone" class="isDone">✅</button>
              <span class="project-task">
                {{ item.title }}
              </span>
            </div>
            <div>
              <span class="project-deadline">
                {{ item.deadLine }}
              </span>
              <span class="project-title">
                {{ item.projectTitle }}
              </span>
              <button v-on:click="deleteItem(index)" class="delete">🗑</button>
            </div>
          </li>
        </ul>
      </div>

      <div v-else :class="{isDoneList: isShowIsDoneList}">
        <ul v-if="items.length">
          <li v-for="(item, index) in narrowDownCompleteItem" v-bind:key="index">
            <div>
              <button v-on:click="item.isDone = !item.isDone" class="isDone">✅</button>
              <span class="project-task">
                {{ item.title }}
              </span>
            </div>
            <div>
              <span class="project-deadline">
                {{ item.deadLine }}
              </span>
              <span class="project-title">
                {{ item.projectTitle }}
              </span>
              <button v-on:click="deleteItem(index)" class="delete">🗑</button>
            </div>
          </li>
        </ul>
      </div>
      <!-- モーダルで表示 -->
      <div class="add-area">
        <Addition v-if="isAdditionalViewShow"></Addition>
      </div>
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
        isSelectIsDoneBtn: true,
        isActiveIncompleteList: true,
      }
    },
    components: {
      Header,
      Addition
      // インポートの時に設定した名前
    },
    methods: {
      switchTabs(isClickedIncompleteTab) {
        this.isActiveIncompleteList = isClickedIncompleteTab;
      },
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
        this.saveItemsToLocalstorage();
      },
      deep: true
    },
    mounted() {
      this.items = JSON.parse(localStorage.getItem('items')) || [];
    },
    computed: {
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
    text-align: center;
  }

  html,
  body {
    height: 100%;
  }

  body {
    background-color: white;
  }

  .tabMenu button {
    background-color: #845EC2;
    padding: 10px;
    border-radius: 30px;
    margin: 8px;
    width: 100px;
    margin: 20px 0 0 20px;
    font-weight: bold;
    color: white;
    cursor: pointer;
  }

  .tabMenu button:hover {
    background-color: white;
    padding: 9px;
    border-radius: 30px;
    border: 1px solid;
    border-color: #845EC2;
    margin: 8px;
    width: 100px;
    margin: 20px 0 0 20px;
    font-weight: bold;
    color: #845EC2;
  }

  .add-button button {
    position: fixed;
    right: 50px;
    bottom: 50px;
    background-color: #845EC2;
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

  .delete {
    cursor: pointer;
    width: 20px;
  }
  
  .toDoList li {
    background-color: white;
    padding: 30px;
    list-style: none;
    margin: 20px auto;
    border-top: .1px solid lightgray;
    border-bottom: .1px solid lightgray;
    display: flex;
    justify-content: space-between;
  }

  .isDoneList li {
    background-color: white;
    padding: 30px;
    list-style: none;
    margin: 20px auto;
    border-top: .1px solid lightgray;
    border-bottom: .1px solid lightgray;
    display: flex;
    justify-content: space-between;
  }

  .items p {
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
    border: solid 1px #ccc;
    margin: 0 0 10px;
  }

  .items-part input {
    width: 200px;
  }

  .items-part select {
    width: 50px;
    color: gray;
  }

  .items-part label {
    display: block;
    color: black;
  }

  .buttons button {
    font-size: 8px;
    background-color: #845EC2;
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
    font-size: 10px;
    background: rgba(0, 201, 183, 0.8);
    color: white;
    padding: 10px;
    border-radius: 30px;
  }

  .project-deadline {
    color: gray;
    font-size: 10px;
  }

  .tabMenu .activeTab {
    background-color: white;
    padding: 9px;
    border-radius: 30px;
    border: 1px solid;
    border-color: #845EC2;
    margin: 8px;
    width: 100px;
    margin: 20px 0 0 20px;
    font-weight: bold;
    color: #845EC2;
  }

</style>
