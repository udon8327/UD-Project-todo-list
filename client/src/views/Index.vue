<template lang="pug">
#index
  .title-area
    h6 待辦事項
  .button-area
    ud-button(@click="createTodo" type="primary") 新增
  .todo-area
    .todo(v-for="todo in todoList" :key="todo.id")
      p.title {{ todo.title }}
      p.status(:class="{ red: todo.status === 'todo' }") {{ todo.status === "todo" ? "未完成" : "完成" }}
      ud-button(@click="editTodo(todo._id)" type="primary") 編輯
      ud-button(@click="deleteTodo(todo._id)" type="primary") 刪除
  .search-area
    h6 搜尋
    ud-input.mb-2(v-model="search.title" placeholder="請輸入標題")
    ud-radio(v-model="search.status" :options="statusOptions" flex)
  .button-area
    ud-button(@click="resetSearch" type="primary" plain) 重置
    ud-button(@click="getTodoList" type="primary") 查詢
</template>

<script>
export default {
  name: "Index",
  components: {},
  data() {
    return {
      search: {
        title: null,
        status: null,
      },
      statusOptions: [
        { label: "全部", value: null },
        { label: "完成", value: "complete" },
        { label: "未完成", value: "todo" },
      ],
      todoList: [],
    };
  },
  mounted() {
    this.getTodoList();
  },
  methods: {
    getTodoList() {
      this.udAxios
        .get(`/todo`, {
          params: this.search,
        })
        .then((res) => {
          this.todoList = res.data;
        });
    },
    resetSearch() {
      this.search = {
        title: null,
        status: null,
      };
      this.getTodoList();
    },
    createTodo() {
      this.$router.push(`/create`);
    },
    editTodo(id) {
      this.$router.push(`/edit/${id}`);
    },
    deleteTodo(id) {
      this.udAxios
        .delete(`/todo/delete/${id}`)
        .then((res) => {
          this.getTodoList();
        });
    },
    toUrl(url) {
      this.$router.push(url);
    },
  },
};
</script>

<style lang="sass" scoped>
.title-area
  padding: 15px 10% 0 10%

.todo-area
  padding: 15px 10%
  .todo
    display: flex
    justify-content: space-between
    border-bottom: 1px solid #ccc
    padding: 2px 0
    p
      &.status
        color: green
        &.red
          color: red

.button-area
  padding: 15px 10%
  display: flex
  justify-content: space-between
  .ud-button
    flex: 0 0 48%

.search-area
  padding: 0 10%
  h6
    margin-bottom: 15px
</style>
