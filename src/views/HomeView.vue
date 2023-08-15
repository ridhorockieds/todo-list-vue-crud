<template>
    <div class="todolist">
        <h1 class="text-center">Todo List</h1>

        <div class="todolist__add">
            <input type="text" v-model="newTodo" placeholder="Add a todo">
            <button @click="addTodo" class="btn btn-primary">Add</button>
        </div>

        <div class="todolist__list">
            <div class="todolist__item" v-for="(todo, index) in todos" :key="index">
                {{ index + 1 }}.
                <input type="checkbox" v-model="todo.done">
                <span v-if="editedIndex !== index" :class="{ 'todolist__item--done': todo.done }" @click="detailView(index)">{{ todo.text }}</span>
                <input type="text" v-else v-model="editedTodo" @keyup.enter="updateTodo" @keyup.esc="editedIndex = null">
                <div class="todolist__item--right">
                    <button @click="editTodo(index)" class="btn btn-warning">Edit</button>
                    <button @click="removeTodo(index)" class="btn btn-danger">Remove</button>
                </div>
            </div>
        </div>

        <div class="todolist__message">
            <h2 class="text-center">{{message}}</h2>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TodoListView',
    data() {
        return {
            newTodo: '',
            editedIndex: -1,
            editedTodo: '',
            todos: [
                {
                    text: 'Learn Vue.js',
                    done: false
                },
                {
                    text: 'Learn React.js',
                    done: false
                },
                {
                    text: 'Learn Angular.js',
                    done: false
                },
                {
                    text: 'Learn Node.js',
                    done: false
                }
            ]
        }
    },
    methods: {
        addTodo() {
            if(this.newTodo.trim() === '') {
                alert('Please enter a todo!');
            } else if (this.todos.filter(todo => todo.text === this.newTodo).length > 0) {
                alert('Todo already exists!');
            } else {
                this.todos.push({
                    text: this.newTodo,
                    done: false
                });
                this.newTodo = '';
            }
        },
        editTodo(index) {
            this.editedIndex = index;
            this.editedTodo = this.todos[index].text;
        },
        updateTodo() {
            if (this.editedTodo.trim() !== '') {
                this.todos[this.editedIndex].text = this.editedTodo;
                this.editedTodo = '';
                this.editedIndex = -1;
            } else {
                alert('Please enter a todo!');
            }
        },
        removeTodo(index) {
            if(confirm('Are you sure you want to remove this todo?')) {
                this.todos.splice(index, 1);
            }
        }
    },
    computed: {
        message() {
            if(this.todos.length == 0) {
                return 'No todos yet!';
            } else if (this.todos.length >= 4) {
                return 'Hebat!';
            } else {
                return '';
            }
        }
    }
}
</script>

<style scoped>
.text-center {
    text-align: center;
}

.btn {
    padding: 5px 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-size: 16px;
    margin-left: 10px;
}

.btn-primary {
    background-color: #42b983;
    color: #fff;
}

.btn-primary:hover {
    background-color: #3da370;
}

.btn-warning {
    background-color: #f0ad4e;
    color: #fff;
}

.btn-warning:hover {
    background-color: #ec971f;
}

.btn-danger {
    background-color: #f44336;
    color: #fff;
}

.btn-danger:hover {
    background-color: #ef2f2f;
}

.todolist__item {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.todolist__item--right {
  margin-left: auto;
}

.todolist__add {
    padding-bottom: 30px;
    display: flex;
    align-items: center;
}

.todolist__add input {
    padding: 5px 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-size: 16px;
    width: 100%;
}

.todolist__item {
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    margin-bottom: 10px;
}

.todolist__item--done {
    text-decoration: line-through;
}

.todolist__message {
    padding: 30px 0;
}

.todolist__item input[type="text"] {
    width: max-content;
    padding: 5px 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-size: 16px;
}

</style>
