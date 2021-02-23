<template>
  <div id="app">
    <header>
      <h1>ToDoリスト</h1>
    </header>

    <div class="container">
      <!-- radioボタンの作成 -->
      <section>
        <input type="radio" name="status" value="All" v-model="radioBtn">すべて
        <input type="radio" name="status" value="Working" v-model="radioBtn">作業中
        <input type="radio" name="status" value="Done" v-model="radioBtn">完了
      </section>
      <!-- ステータス等表示 -->
      <section>
          <table>
            <tr>
              <th>ID</th>
              <th>コメント</th>
              <th>状態</th>
            </tr>
          </table>
          <!-- 入力された値を表示する -->
          <table>
            <tr v-for="(value, index) in filterTodos" :key="value.id">
              <td>{{ index }}</td>
              <td>{{ value.todo }}</td>
              <td><button @click="statusChange(value.id)">{{ value.status }}</button></td>
              <td><button @click="deleteButton(value.id)">削除</button></td>
            </tr>
          </table>
      </section>
      <section>
        <h1>新規タスクの追加</h1>
        <input type="text" ref="inputFocus" v-model.trim="inputValue">
        <button type="button" @click="addTask">追加</button>
      </section>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      taskList: [],
      inputValue: '',
      radioBtn: ''
    }
  },
  computed: {
    filterTodos() {
      switch (this.radioBtn) {
        case 'Working':
          return this.taskList.filter((todos) => {
            return todos.status === '作業中';
          })
        case 'Done':
          return this.taskList.filter((todos) => {
            return todos.status === '完了';
          })
        default:
          return this.taskList;
      }
    }
  },
  methods: {
  // タスクの追加処理
  addTask() {
    const todos = {
      id: this.taskList.length,
      todo: this.inputValue,
      status: '作業中'
    }
    this.taskList.push(todos);
    this.inputValue = '';
    this.$refs.inputFocus.focus();
  },
  //削除Button押下際の処理
  deleteButton(index) {
    this.taskList.splice(index, 1);
    this.idReset(this.taskList);
  },
  //id値をリセット
  idReset(result) {
    for(let i = 0; i < result.length; i++) {
      result[i].id = i;
    }
      return;
    },
    //ステータスButtonの変更
  statusChange(index) {
    const todo = this.taskList[index];
    if(todo.status === '作業中') {
      todo.status = '完了';
    } else if (todo.status === '完了') {
      todo.status = '作業中';
    }
  },
}
}
</script>

