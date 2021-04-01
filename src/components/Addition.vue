<template>
  <div class="items-part">
    <form v-on:submit.prevent>
      <label>Project title
        <input type="text" v-model="newItem.projectTitle">
      </label>
      <label>Task
        <input type="text" v-model="newItem.title">
      </label>
      <label>Deadline
        <input type="date" value="00:00" v-model="newItem.deadLine">
      </label>
      <label>Priority lebel
        <select name="importance" size="1" v-model="newItem.importance">
          <option value='' disabled selected style='display:none;'>Select</option>
          <option value="1">High</option>
          <option value="2">Medium</option>
          <option value="3">Low</option>
        </select>
      </label>
      <div class="buttons">
        <button v-on:click="addItem">Create</button>
        <button v-on:click="addCancel">Cancel</button>
      </div>
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
          projectTitle: "",
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
        let diff = (deadline - today) / (24 * 60 * 60 * 1000);
        return diff;
      },
      addCancel() {
        this.$parent.addShow = false;
      },
      triggerEvent() {
        // this.$emit('add-event');
      },
      addItem() {
        var item = {
          projectTitle: this.newItem.projectTitle,
          title: this.newItem.title,
          deadLine: this.newItem.deadLine,
          importance: this.newItem.importance,
          priority: this.diffDeadlineAndToday() * this.newItem.importance,
          isDone: false
        };
        this.$parent.items.push(item);
        this.newItem = '';
        this.$parent.addShow = false;
        this.$parent.saveItemsToLocalstorage();
      },
    },
    computed: {}
    // 締め切りと今日の差分
  };

</script>
