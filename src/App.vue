<template>
  <div class="container">
    <h2>To-Do List</h2>
    <form @submit.prevent="onSubmit">
      <div class="d-flex">
        <div class="flex-grow-1 mr-3">
          <input 
          class="form-control"
          type="text" 
          v-model="todo"
          placeholder="Type new to-do"
          >
        </div>
        
        <div>
          <button 
          type="submit"
          class="btn btn-primary"
          >
          Add
          </button>
        </div>
      </div>
    
      <div v-show="hasError" style="color: red">
        This field cannot be empty
      </div>
    </form>
    <div v-if="!todos.length">
      추가된 Todo가 없습니다.
    </div>
    <div 
      v-for="(todo, index) in todos"
      :key="todo.id"
      class="card mt-2"
    >
      <div class="card-body p-2 d-flex align-items-center">
        <div class="form-check flex-grow-1">
          <input
            class="form-check-input"
            type="checkbox"
            v-model="todo.completed"
          >
          <label 
            class="form-check-label"
            :class="{todo: todo.completed}"
          >
            {{ todo.subject }}
          </label>
        </div>

        <div>
          <button 
            class="btn btn-danger btn-sm"
            @click="deleteTodo(index)"
          >
            Delete
          </button>
        </div>

      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'; //ref or reactive
/*
 * v-bind:class는 :class으로 생략이 가능함
 * v-on:click는 @click로 생략이 가능함
 * v-model => 양방향 바인딩
 * v-for 반복문 사용시 :key값 필수적
 * v-if
 * v-show
 * v-model
 * 
*/
export default {
  setup() {
    //ref사용할때 변수의 값을 변경하려면 변수명.value를 통해서 값을 변경해야함
    const toggle = ref(false);
    const todo = ref('');
    const todos = ref([]);
    const hasError = ref(false);
    const todoStyle = {
      textDecoraton: 'line-through',
      color: 'gray'
    };

    const onSubmit = () => {
      if (todo.value === '') {
        hasError.value = true;
      } else { 
        todos.value.push({
          id: Date.now(),
          subject: todo.value,
          completed: false,
        });
        hasError.value = false;
        todo.value = '';
      }
    };

    const onToggle = () => {
      toggle.value = !toggle.value;
    };

    const deleteTodo = (index) => {
      todos.value.splice(index, 1);
      console.log(index);
    };

    return {
      todo,
      todos,
      onSubmit,
      toggle,
      onToggle,
      hasError,
      todoStyle,
      deleteTodo,
    };
  }
}
</script>

<style>
  .todo {
    color: gray;
    text-decoration: line-through;
  }

</style>