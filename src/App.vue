<template>
  <div id="app">
      <div class="container">
        <nav>
          <TodoCategoryList v-bind:propscategory="categoryItems" v-on:changeCategory="changeCategory"></TodoCategoryList>
        </nav>
        <section>
          <TodoHeader></TodoHeader>
          <TodoInput v-on:addTodo="addTodo"></TodoInput>
          <TodoList v-bind:propsdata="todoItems" v-on:removeTodo="removeTodo"></TodoList>
          <TodoFooter v-on:removeAll="clearAll"></TodoFooter>
        </section>
      </div>    
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'
import TodoCategoryList from './components/TodoCategoryList.vue'

export default {
  data() {
    return {
      todoItems:[],
      categoryItems:[],
      selectedCategory:0
    }
  },
  created() {
      var category = this.getCategory();
      this.categoryItems.push(...category);
      this.changeCategory(this.selectedCategory);   
  },
  methods: {
    getCategory() {
      if(localStorage.getItem("category") !== null) {
        return JSON.parse(localStorage.getItem("category"));
      }
    },
    addTodo(todoItem){
      // 로컬 스토리지에 데이터를 추가하는 로직
      if(localStorage.getItem(this.selectedCategory) !== null) {
        var selectedCategroyTodolist = JSON.parse(localStorage.getItem(this.selectedCategory));
        selectedCategroyTodolist.push(todoItem);
        localStorage.setItem(this.selectedCategory, JSON.stringify(selectedCategroyTodolist));
        this.todoItems.push(todoItem);
      }
    },
    clearAll() {
      // 카테고리 제외하고 clear
      var category = this.getCategory();
      localStorage.removeItem(...category);
      this.todoItems=[];
    },
    removeTodo(todoItem, index){

      if(localStorage.getItem(this.selectedCategory) !== null) {
         var todolist = JSON.parse(localStorage.getItem(this.selectedCategory));
         todolist.splice(index,1);
         this.todoItems.splice(index, 1);
         localStorage.setItem(this.selectedCategory, JSON.stringify(todolist));      
       }
    },
    changeCategory(index){
      this.selectedCategory = index;

      if(localStorage.getItem(index) !== null) {
        var todolist = JSON.parse(localStorage.getItem(index));
        this.todoItems = [];
        this.todoItems.push(...todolist);
      }
    }
  },
  components: {
    'TodoHeader' : TodoHeader,
    'TodoInput' : TodoInput,
    'TodoList' : TodoList,
    'TodoFooter' : TodoFooter,
    'TodoCategoryList' : TodoCategoryList
  }
}
</script>

<style>
html{height: 100%;}
#app{height: 100%;}
body{margin: 0;text-align: center;background-color: #F6F6F8;height: 100%;} 
.container{display: flex; flex-wrap: nowrap;height: 100%;}
.container nav {flex-grow: 1; flex-basis: 25%;}
.container section {flex-grow: 2; flex-basis: 75%;margin: 0 2%;}
input{border-style: groove;width: 200px;}
button{border-style: groove;}
.shadow{box-shadow: 5px 10px 10px rgba(0,0,0,0.03)}
</style>
