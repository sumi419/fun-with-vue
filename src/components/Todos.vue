<template>
  <div class="hello">
    <div class="holder">

      <form @submit.prevent="addTodo">
      <!-- similar to prevent default in react
       uses the addTodo method we have in our script tags below-->
      <input type="text" placeholder="Enter a Todo.." v-model="todo" v-validate="'min:5'" name="todo">
      <!-- v-model used for two way binding of input to the todo from data -->

      <transition name="alert-in">
        <p class="alert" v-if="errors.has('todo')">{{ errors.first('todo') }}</p>
        <!-- v-if is an if statement 
        {{}} indicates javascript -->
      </transition>
      </form>

      <ul>
        <transition-group name="list" enter-active-class="animated bounceInUp" leave-active-class="animated bounceOutDown">
          <li v-for="(data, index) in todos" :key="index">
          <!-- v-for is the same as a for in loop (like mapping) -->
            {{ data.todo }}
            <i class="fa fa-minus-circle" v-on:click="remove(index)"></i>
            <!-- v-on:click same as onClick() method -->
          </li>
        </transition-group>
      </ul>
      <p>These are your todos</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Todos',
  data() {
    return {
      todo: '',
      todos: [
        { "todo": "buy things"},
        { "todo": "by more fings"},
      ]
    }
  },
  methods: {
    addTodo() {
      this.$validator.validateAll().then((res) => {
        if (res && this.todo !== '') {
          this.todos.push({todo: this.todo})
          this.todo = ''
        } else if (this.todo === '') {
          alert('please enter in a todo')
        } else {
          alert('Not Long Enough!')
        }
      })
    },
    remove(id) {
      this.todos.splice(id, 1)
    }
  }
}
</script>

  <!-- 
  name of the script would be the componet you are exporting
  data is similar to state in react
  reset the state by setting todo to an empty string every time after you add a new todo
  -->

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

@import "https://cdn.jsdelivr.net/npm/animate.css@3.5.1";
@import "https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css";

.holder {
    background: #fff;
  }

  ul {
    margin: 0;
    padding: 0;
    list-style-type: none;
  }
  
  ul li {
    padding: 20px;
    font-size: 1.3em;
    background-color: #E0EDF4;
    border-left: 5px solid #3EB3F6;
    margin-bottom: 2px;
    color: #3E5252;
  }

  p {
    text-align:center;
    padding: 30px 0;
    color: gray;
  }

  .container {
    box-shadow: 0px 0px 40px lightgray;
  }

  input {
    width: calc(100% - 40px);
    border: 0;
    padding: 20px;
    font-size: 1.3em;
    background-color: #323333;
    color: #687F7F;
  }

  .alert {
    background: #fdf2ce;
    font-weight: bold;
    display: inline-block;
    padding: 5px;
    margin-top: -20px;
  }

  .alert-in-enter-active {
    animation: bounce-in .4s;
  }

  .alert-in-leave-active {
    animation: bounce-in .4s reverse;
  }

  @keyframes bounce-in {
    0% {
      transform: scale(0);
    }
    50% {
      transform: scale(1.5);
    }
    100% {
      transform: scale(1);
    }
  }

  i {
    float: right;
    cursor: pointer;
  }
</style>

<!-- template tag is your html -->
<!-- script tag is your javascript -->
<!-- style tag is your css -->