<script>
export default {
  data() {
    return {
      todos: [],
      New: "",
    };
  },
  methods: {
    add() {
      this.todos.push(this.New);
      console.log(this.New);
    },
  },
};
</script>

<template>
  <header>
    <div>
      <h1>TODO</h1>
      <img src="../assets/icon-moon.svg" alt="moon" id="toggle" />
    </div>
    <div id="push">
      <input
        type="text"
        id="input"
        v-model="New"
        @keyup.enter="add"
        placeholder="Enter To-Do"
      />
      <button @click="add">Add Todo</button>
    </div>
  </header>
  <div class="main">
    <!--h3>To-do List</h3-->
    <div class="host">
      <div id="todo" v-for="todo in todos" :key="todo">
        <!--hr /-->
        <p>
          <input type="checkbox" id="radio" />
          <span id="norm" >{{ todo }}</span>
          <span id="not">{{ checkbox }}</span>
          <img src="../assets/icon-cross.svg" id="right" />
        </p>
      </div>
    </div>
    <!--input
      type="text"
      v-model="New.Title"
      @keyup.enter="add"
      placeholder="Enter To-Do"
    /-->
    <br /><br />
    <!--input type="time" v-model="New.Time" @keyup.enter='add' placeholder='Enter time'/-->
  </div>
</template>

<style></style>
