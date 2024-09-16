<script>
export default {
    name: 'TodoItem',
    
    props: ['filteredTodos', 'editedTodo'],

    data: () => ({}),

    watch: {},

    mounted() {

    },

    computed: {

    },

    methods: {
        editTodo(todo) {
            this.$emit('editTodo', todo);
        },
        removeTodo(todo) {
            this.$emit('removeTodo', todo);
        },
        cancelEdit(todo) {
            this.$emit('cancelEdit', todo);
        },
        doneEdit(todo) {
            this.$emit('doneEdit', todo);
        }
    }
}
</script>

<template>
    <li v-for="todo in filteredTodos" :key="todo.id" class="todo-item" :class="{ completed: todo.completed, editing: todo === editedTodo }">
        <div class="view">
            <input class="toggle" type="checkbox" v-model="todo.completed">
            <label @dblclick="editTodo(todo)">{{ todo.title }}</label>
            <button class="destroy" @click="removeTodo(todo)"></button>
        </div>

        <input
            @vnode-mounted="({ el }) => el.focus()"
            v-if="todo === editedTodo"
            class="edit"
            type="text"
            v-model="todo.title"
            @keyup.escape="cancelEdit(todo)"
            @blur="doneEdit(todo)"
            @keyup.enter="doneEdit(todo)"
        >
    </li>
</template>
