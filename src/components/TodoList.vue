<template>
  <div>
      <input type="text" class="todo-input" placeholder="what needs to be done" v-model="newTodo" @keyup.enter="addTodo">
      <transition-group name='fade' enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
        <todo-item v-for="todo in todosFiltered" :key="todo.id" :todo="todo" :checkAll="!anyRemaining">
        </todo-item>
      </transition-group>
      <div class="extra-container">
          <todo-check-all></todo-check-all>
          <!-- <div><label><input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos"> Check All</label></div> -->
          <!-- <div>{{ remaining }} item left</div> -->
          <todo-items-remaining></todo-items-remaining>
      </div>

      <div class="extra-container">
          <todo-filtered></todo-filtered>

        <div>
            <transition name="fade">
                <todo-clear-completed></todo-clear-completed>
            </transition>
        </div>
      </div>

  </div> 
</template>

<script>

import TodoItem from './TodoItem'
import TodoItemsRemaining from './TodoItemsRemaining'
import TodoCheckAll from './TodoCheckAll'
import TodoFiltered from './TodoFiltered'
import TodoClearCompleted from './TodoClearCompleted'

export default {
  name: 'todo-list',
  components: {
      TodoItem,
      TodoItemsRemaining,
      TodoCheckAll,
      TodoFiltered,
      TodoClearCompleted,
  },
  data () {
    return {
      newTodo: '',
      idForTodo: 3,
    }
  },
/*   created(){
      eventBus.$on('removedTodo', (id) => this.removeTodo(id))
      eventBus.$on('finishedEdit',(data) => this.finishedEdit(data))
      eventBus.$on('checkAllChanged', (checked) => this.checkAllTodos(checked))
      eventBus.$on('filterChanged', (filter) => this.filter = filter) 
      eventBus.$on('clearCompletedTodos', () => this.clearCompleted()) 
  },
  beforeDestroy(){
      eventBus.$off('removedTodo')
      eventBus.$off('finishedEdit')
      eventBus.$off('checkAllChanged')
      eventBus.$off('filterChanged')
      eventBus.$off('clearCompletedTodos')
  }, */
  computed:{
      anyRemaining(){
          return this.$store.getters.anyRemaining
      },
      todosFiltered(){
          return this.$store.getters.todosFiltered
      },
  },

  methods: {
      addTodo(){
          if(this.newTodo.trim().length == 0){
              return 
          }
          this.$store.dispatch('addTodo',{
              id: this.idForTodo,
              title: this.newTodo,
          })
          this.newTodo = ''
          this.idForTodo++
      },
  }
}
</script>

<style>

    @import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");
    
    .todo-input{
        width:100%;
        padding: 10px 18px;
        font-size: 18px;
        margin-bottom: 16px;
    }

    .todo-item{
        margin-bottom: 12px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        animation-duration: 0.3s;
    }

    .remove-item{
        cursor: pointer;
        margin-left: 14px;
        
    }

    .todo-item-left{
        display: flex;
        align-items: center;
    }

    .todo-item-label{
        padding: 10px;
        border: 1px solid white;
        margin-left: 12px;
    }
    .todo-item-edit{
        font-size: 24px;
        color: #2c3e50;
        margin-left: 12px;
        width: 100%;
        padding: 10px;
        border: 1px solid #ccc;
        font-family: 'Avenir', Arial, Helvetica, sans-serif;
    }
    .completed{
        text-decoration: line-through;
        color: gray;
    }


    .extra-container {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        border-top: 1px solid lightgrey;
        padding-top: 14px;
        margin-bottom: 14px;
    }

    button{
        font-size: 14px;
        background-color: white;
        appearance: none;
    }


    .active {
        background: lightgreen;
    }
 
    .fade-enter-active, .fade-leave-active {
        transition: opacity .2s;
    }
    .fade-enter, .fade-leave-to {
        opacity: 0;
    }

</style>
