<script>
import { ref } from "vue";
export default {
    setup() {
        const newTodo = ref('');
        const todosData = JSON.parse(localStorage.getItem('todos'));
        const todos = ref(todosData);
        function addTodo() {
            if (newTodo.value) {
                todos.value.push({
                    done: false,
                    content: newTodo.value
                });
                newTodo.value = '';
            }
            saveData();
        }
        function doneTodo(todo) {
            todo.done = !todo.done
            saveData();
        }
        function removeTodo(index) {
            todos.value.splice(index, 1);
            saveData();
        }
        function saveData() {
            const storageData = JSON.stringify(todos.value);
            localStorage.setItem('todos', storageData);
        }
        return {
            todos,
            newTodo,
            addTodo,
            doneTodo,
            removeTodo,
            saveData
        }
    }
}
</script>
<template>
    <div class="container">
        <div class="row">
            <div class="upper col-12">
                <img src="./../assets/vue.svg" class="logo vue" alt="Vue logo" />
                <h1>Todo List App</h1>
                <h3>{{ newTodo }}</h3>
            </div>
            <div class="col-6 ">
                <form @submit.prevent="addTodo()">
                    <input type="text" v-model="newTodo" name="newTodo" class="form-control mb-2" placeholder="Name Todos">
                    <button class="btn-primary">Add Todo</button>
                </form>
            </div>
            <div class="col-6 ">
                <ul>
                    <li v-for="(todo, index) in todos" :key="index">
                        <span :class="{ done: todo.done }" @click="doneTodo(todo)">{{ todo.content }}</span>
                        <button class="btn-remove mx-2" @click="removeTodo(index)"><i class="fa fa-trash"></i></button>
                    </li>
                    <div class="d-grid gap-2">
                        <button class=" btn-block empty" v-if="todos.length === 0">List Empty</button>
                    </div>
                </ul>
            </div>
        </div>
    </div>
</template>