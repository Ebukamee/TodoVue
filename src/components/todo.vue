<script>
  export default {
    mounted() {
      const changeTheme = this.getTheme() || this.getPreference();
      this.setTheme(changeTheme);
      this.todos =this.getTodos()
      this.todos = this.todos.filter((todo) => todo.name!=="");

    },
    watch: {
      todos: {
        handler() {
          this.setTodos(this.todos);
        },
        deep:true
      }
    },
    data() {
      return {
        New: "",
        todos: [
          {
            id: 1,
            name: "",
            IsCompleted: false,
          },
        ],
        Dark: '',
      };
    },
    methods: {
      setTodos(todos) {
        localStorage.setItem("todos", JSON.stringify(todos));
      },
      getTodos(todos) {
        const storedTodos = localStorage.getItem("todos");

        if (storedTodos) {
          return JSON.parse(storedTodos);
        }
        return[]
      },
      add() {
        if (this.todos.length >= 7) {
          alert("Finish Your Todos First");
        }
        else if (!this.New) {
          this.New=""
        }
        else {
          this.todos.push({
            id: this.todos.length + 1,
            name: this.New,
            IsCompleted: false,
          });
          this.New = "";
        }
      },

      Ondelete(id) {
        this.todos = this.todos.filter((todo) => todo.id!==id);
      },

      toggle() {
        const active = localStorage.getItem("user-theme");
        if (active === "light-theme") {
          this.setTheme("dark-theme");
          this.Dark = true;
        } else {
          this.setTheme("light-theme");
          this.Dark = false;
        }
      },
      getTheme() {
        return localStorage.getItem("user-theme");
      },
      setTheme(theme) {
        localStorage.setItem("user-theme", theme);
        this.userTheme = theme;
        document.documentElement.className = theme;
      },
      getPreference() {
        const DarkPre = window.matchMedia(
          "(prefers-color-scheme : dark)"
        ).matches;
        if (DarkPre) {
          return "dark-theme";
        } else {
          return "light-theme";
        }
      },
      onComplete(todo) {
        todo.IsCompleted=!todo.IsCompleted
      },
      onClear(todo) {
       this.todos= this.todos.filter((todo) => todo.IsCompleted!==true);
      }
    },
  };
</script>

<template>
  <header>
    <div>
      <h1>TODO</h1>
      <img
        src="../assets/icon-sun.svg"
        alt="sun"
        id="toggle"
        @click="toggle"
        v-show="Dark===true"
      />
      <img
        src="../assets/icon-moon.svg"
        alt="moon"
        id="toggle"
        @click="toggle"
        v-show="!Dark"
      />
    </div>
    <div id="push">
      <input
        type="text"
        id="input"
        v-model="New"
        @keyup.enter="add"
        placeholder="Enter To-Do"
      />
      <button @click="add" :disabled="!New" :class="{'disable':!New }">
        Add Todo
      </button>
    </div>
  </header>
  <div class="main">
    <div class="host">
      <div id="todo" v-for="(todo,i) in todos" :key="i" :todo="New">
        <div class="flex">
          <div id="radio" @click="onComplete(todo)" :class="{'complete':todo.IsCompleted}">
           <img src='../assets/icon-check.svg' alt='check' v-show='todo.IsCompleted' class='checked'/>
          </div>
          <p id="norm" :class="{'linethrough':todo.IsCompleted}">{{ todo.name }}</p>
          <img src="../assets/icon-cross.svg" id="right" @click="Ondelete(todo.id)" />
        </div>
      </div>
      <div class='bottom'>
        <p>{{ todos.length }} Items Available, {{ todos.filter(todo => !todo.IsCompleted).length }} {{  todos.filter(todo => !todo.IsCompleted).length<=1 ? "Item" : "Items" }} left</p>
        <button @click='onClear(todo)' id='clear'>Clear Completed</button>
      </div>
    </div>
    
  </div>
</template>

<style></style>
