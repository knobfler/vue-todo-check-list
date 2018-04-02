<template>
<div class="todo-wrapper">
    <div class="title">Todo's Home.</div>
    <div class="todos">
        <div class="todo-contents">
            <input
                type="text"
                name="todo"
                class="inputTodo"
                v-model="todoText"
                v-on:keydown.enter="addTodo"
                placeholder="투두를 입력하세요."/>
            <div class="todo-list" v-for="todo in this.todoList.slice().reverse()">
                <div class="todo">
                    <div class="todo-check">
                        <div v-if="todo.check === true">
                            ✓
                        </div>
                    </div>
                    <div class="todo-text"  v-on:click="check(todo.id)">{{todo.text}}</div>
                    <div class="xButton" v-on:click="removeTodo(todo.id)">X</div>
                </div>                        
            </div>
            <div class="buttons">
                <button class="remove-button" v-on:click="removeChecked">Remove Checked</button>
            </div>  
        </div>
    </div>
</div>
</template>

<script>
export default {
  name: 'Home',
  data() {
      return {
          id: 0,
          todoList: [],
          todoText: ''
      }
  },
  created() {
      this.initialize();
  },
  methods: {
      initialize() {
          if(localStorage.todoList) {
              this.todoList = JSON.parse(localStorage.todoList);
                const lastIndex = JSON.parse(localStorage.todoList).length;
                console.log(lastIndex);
                this.id = lastIndex;
          }
      },
      addTodo() {
          const todo = 
          this.todoList = this.todoList.concat({
            id: this.id++,
            text: this.todoText,
            check: false
          });
          console.log(this.todoList);
          localStorage.todoList = JSON.stringify(this.todoList);
          this.todoText = '';
      },
      removeTodo(id) {
          console.log("remove");
          this.todoList = this.todoList.filter(
              todo => todo.id !== id
          );
          localStorage.todoList = JSON.stringify(this.todoList);
      },
      check(id){
          const index = this.todoList.findIndex(todo => todo.id === id);
          console.log(index);
          const selected = this.todoList[index];
          const nextTodos = [...this.todoList];
          nextTodos[index] = {
              ...selected,
              check: !selected.check
          }
          this.todoList = nextTodos;
          localStorage.todoList = JSON.stringify(this.todoList);
      },
      removeChecked() {
          this.todoList = this.todoList.filter(
              todo => todo.check === false
          );
          console.log(this.todoList);
          localStorage.todoList = JSON.stringify(this.todoList);
      }
  }
}
</script>

<style>
.todo-wrapper {
    background: #f1f3f5;
    margin: 0 auto;
    width: 50%;
}
.title {
    text-align: center;
}

.todos {

}

.todo-contents {

}

.inputTodo {
    border: none;
    outline: none;
    width: 100%;
    background: #868e96;
    color: white;
    height: 2rem;
    font-size: 1.5rem;
}

::placeholder { /* Chrome, Firefox, Opera, Safari 10.1+ */
    color: white;
    opacity: 1; /* Firefox */
}

:-ms-input-placeholder { /* Internet Explorer 10-11 */
    color: white;
}

::-ms-input-placeholder { /* Microsoft Edge */
    color: white;
}

.todo-list {
    display: flex;
    flex-direction: column;
    /* align-items: center;
    justify-content: center; */
}

.todo {
    display:flex;
    flex-direction: row;
    padding-top: 1rem;
    padding-left: 4rem;
    padding-right: 4rem;
    height: 2.5rem;
    font-weight: 600;
    font-size: 1.5rem;

   
}

.todo-text {
    flex: 1;
    cursor: pointer;
}

.xButton {
    margin-left: auto;
    cursor: pointer;
}

.todo-check {
    padding-right: 4rem;
}

.buttons {
    display: flex;
    align-items: center;
    justify-content: center;
}
.remove-button{
    color: white;
    font-weight: 600;
    height: 2rem;
    border-radius: 3px;
    border: 1px solid white;
    background: green;
}


</style>