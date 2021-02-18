<template>
  <div class="items-part">
    <form v-on:submit.prevent>
      <input type="text" v-model="newItem.title">
      <label>締め切り時間:
        <input type="date" value="00:00" v-model="newItem.deadLine">
      </label>
      <label>見積り時間:
        <input type="time" value="00:00" v-model="newItem.expectedTime">
      </label>
      <input type="submit" v-model="newItem.priority">
      <select name="priority" size="1">
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
      </select>
      <button v-on:click="addItem">create</button>
      <!-- value属性値は、初期入力値となる。
          時間入力欄のvalue属性値は、hh:mm:ssの形式で入力する -->
    </form>
    <ul>
      <li v-for="(item, index) in items" v-bind:key="index">
        {{ item.title }}{{ item.deadLine }}{{ item.expectedTime }}
        <div v-on:click="deleteItem(index)" class="delete">
        </div>
      </li>
      <pre> {{ $data }} </pre>
    </ul>
  </div>
</template>
<script>
  // コンポーネント読み込み
  export default {
    name: 'Addition',
    created() {
      this.triggerEvent();
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
        show: true
      }
    },
    methods: {
      triggerEvent() {
        this.$emit('add-event');
      },
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
    computed: {},
  };

</script>
