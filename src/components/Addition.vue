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
      <button v-on:click="addCancel">cancel</button>
      <!-- value属性値は、初期入力値となる。
          時間入力欄のvalue属性値は、hh:mm:ssの形式で入力する -->
    </form>
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
        addShow: false
      }
    },
    methods: {
      addCancel() {
        this.$parent.addShow = false
      },
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
        this.$parent.items.push(item);
        this.setItems();
        this.newItem = '';
        this.$parent.addShow = false
      },
    },
    computed: {},
  };

</script>
