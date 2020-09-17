<template>
    <div id="app">
        <TodoHeader />
        <TodoInput />
        <TodoList />
        <TodoFooter />
    </div>
</template>

<script>
import TodoHeader from './components/TodoHeader';
import TodoInput from './components/TodoInput';
import TodoList from './components/TodoList';
import TodoFooter from './components/TodoFooter';

export default {
    components: {
        // 컴포넌트 태그명 : 컴포넌트 내용
        TodoHeader: TodoHeader,
        TodoInput: TodoInput,
        TodoList: TodoList,
        TodoFooter: TodoFooter,
    },
};
</script>

<style></style>
