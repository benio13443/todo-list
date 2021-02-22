<template>
  <div class="items-part">
    <form v-on:submit.prevent>
      <input type="text" v-model="newItem.title">
      <label>締め切り時間:
        <input type="date" value="00:00" v-model="newItem.deadLine">
      </label>
      <select name="importance" size="1" v-model="newItem.importance">
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
          importance: "",
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
      addItem() {
        var item = {
          title: this.newItem.title,
          deadLine: this.newItem.deadLine,
          importance: this.newItem.importance,
          isDone: false
        };
        this.$parent.items.push(item);
        this.newItem = '';
        this.$parent.addShow = false
      },
    },
    computed: {},
  };

</script>
