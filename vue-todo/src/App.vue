<template>
    <div id="app">
        <TodoHeader />
        <TodoInput v-on:addTodoItem="addOneItem" />
        <TodoList v-bind:todoItems="todoItems" v-on:removeItem="removeOneItem" v-on:toggleItem="toggleOneItem" />
        <TodoFooter v-on:clearAll="clearAllItems" />
    </div>
</template>

<script>
import TodoHeader from './components/TodoHeader';
import TodoInput from './components/TodoInput';
import TodoList from './components/TodoList';
import TodoFooter from './components/TodoFooter';

export default {
    data() {
        return {
            todoItems: [],
        };
    },
    methods: {
        addOneItem(todoItem) {
            const obj = { completed: false, item: todoItem };
            localStorage.setItem(todoItem, JSON.stringify(obj));
            this.todoItems.push(obj);
        },
        removeOneItem(todoItem, index) {
            localStorage.removeItem(todoItem.item);
            this.todoItems.splice(index, 1);
        },
        toggleOneItem(todoItem, index) {
            this.todoItems[index].completed = !this.todoItems[index].completed;
            localStorage.removeItem(todoItem.item);
            localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
        },
        clearAllItems() {
            localStorage.clear();
            this.todoItems = [];
        },
    },
    created() {
        if (localStorage.length > 0) {
            for (let i = 0; i < localStorage.length; i++) {
                if (localStorage.key(i) !== 'loglevel:webpack-dev-server') {
                    this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
                }
            }
        }
    },
    components: {
        TodoHeader,
        TodoInput,
        TodoList,
        TodoFooter,
    },
};
</script>

<style>
body {
    font-family: Ubuntu;
    text-align: center;
    background-color: #f6f6f6;
}
input {
    border-style: groove;
    width: 200px;
}
button {
    border-style: groove;
}
.shadow {
    box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
}
</style>
