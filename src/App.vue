<script >
import ChildComp from './ChildComp.vue'
let id =0 ;
export default {
  
    components:{
      ChildComp
    },
    data() {
      return {
        todoId: 1,
        message: `I'm dynamic`,
        titleClass: `title`,
        doneClass: `done`,
        hideCompleted : false,
        greeting: 'Hello from parent',
        count: 0,
        text:``,
        awesome:true,
        newtodo:``,
        todos:[{
          id: id++,text :`HTML`,done:true
        },{
          id: id++,text :`CSS`,done:true
        },{
          id: id++,text :`JS`,done:false
        }], 
        childMsg: 'No child msg yet',
        msg: 'from parent'
      }
    }, 
    mounted() {
      // this.$refs.p.textContent = 'mounted!';
      this.fetchData()
    },methods:{
      increase(){
        this.count++;
      },
      onInput(e){
        this.text = e.target.value; 
      },
      addTodo(){
        console.log(`add todo`,this.newtodo)
        this.todos.push({id: id++,text :this.newtodo } );
        this.newtodo = ``;
      },
      deleteTodo(todo){
        this.todos =  this.todos.filter((element) => {   return element != todo; })
      },
      async fetchData(){
        this.todoData = null;
        const res = await fetch(
            `https://jsonplaceholder.typicode.com/todos/${this.todoId}`
          )
          this.todoData = await res.json()
          console.log(this.todoData);
      }
    }, computed: {
    filteredTodos() {
      return this.hideCompleted ? this.todos.filter((element)=>{ return !element.done}) : this.todos 
      // return filtered todos based on `this.hideCompleted`
    }
  }
}
</script>

<template>
  <header>

    <div > <h1 :class="titleClass">{{ message }}</h1></div>
    <button @click="increase">{{ count }}</button>

  </header>
  <main>
    <ChildComp :msg="greeting" /> <br/>
    <!-- <ChildComp  @response="(msg) => childMsg = msg " /> -->
    {{ childMsg }} <br/>
    <ChildComp>Message: {{ msg }}</ChildComp> <br/>
    <input :value="text" @input="onInput"  />
    <p>{{ text }} </p> <br/>
    <h1 v-if="awesome">Vue is awesome!</h1>
    <h1 v-else>Oh no 😢</h1>
  
  
    <form @submit.prevent="addTodo">
      <input v-model="newtodo">
      <button type="submit">Add Todo</button>    
    </form>
    <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.done" />
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
      <button @click="deleteTodo(todo)">X</button>
    </li>
  </ul>

  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Show all' : 'Hide completed' }}
  </button>
    <p ref="p">hello</p>
    <p>Todo id: {{ todoId }}</p>
    <button @click="todoId++">Fetch next todo</button>
    <p v-if="!todoData">Loading...</p>
    <pre v-else>{{ todoData }}</pre>
  </main>
</template>

<style>
.title {
  color: red;
}

.done {
  text-decoration: line-through;
}
</style>