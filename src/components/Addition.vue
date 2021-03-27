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
    data() {
      return {
        addShow: false,
        newItem: {
          title: "",
          deadLine: "",
          importance: ""
        },
      }
    },
    created() {
      this.triggerEvent();
    },
    methods: {
      diffDeadlineAndToday() {
        let deadlineArray = this.newItem.deadLine.split('-');
        let deadline = new Date(deadlineArray[0], deadlineArray[1], deadlineArray[2]);
        let now = new Date(Date.now());
        let today = new Date(now.getFullYear(), now.getMonth() + 1, now.getDate());
        let diff = (deadline - today)/ (24 * 60 * 60 * 1000);
        return diff;
      },
      addCancel() {
        this.$parent.addShow = false
      },
      triggerEvent() {
        // this.$emit('add-event');
      },
      addItem() {
        var item = {
          title: this.newItem.title,
          deadLine: this.newItem.deadLine,
          importance: this.newItem.importance,
          priority: this.diffDeadlineAndToday() * this.newItem.importance,
          isDone: false
        };
        this.$parent.items.push(item);
        this.newItem = '';
        this.$parent.addShow = false
      },
    },
    computed: {}
    // 締め切りと今日の差分
  };

</script>
