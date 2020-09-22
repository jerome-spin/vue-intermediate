<template>
    <div id="app">
        <TodoHeader />
        <TodoInput />
        <TodoList v-bind:todoItems="todoItems" />
        <TodoFooter />
    </div>
</template>

<script>
import TodoHeader from './components/TodoHeader';
import TodoInput from './components/TodoInput';
import TodoList from './components/TodoList';
import TodoFooter from './components/TodoFooter';

export default {
    data: function() {
        return {
            todoItems: [],
        };
    },
    created: function() {
        if (localStorage.length > 0) {
            for (var i = 0; i < localStorage.length; i++) {
                if (localStorage.key(i) !== 'loglevel:webpack-dev-server') {
                    this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
                }
            }
        }
    },
    components: {
        // 컴포넌트 태그명 : 컴포넌트 내용
        TodoHeader: TodoHeader,
        TodoInput: TodoInput,
        TodoList: TodoList,
        TodoFooter: TodoFooter,
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
