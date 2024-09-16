<script>

import TodoItem from './TodoItem.vue';
import TodoFooter from './TodoFooter.vue';

const filters = {
    all: (todos) => todos,
    active: (todos) => todos.filter((todo) => !todo.completed),
    completed: (todos) => todos.filter((todo) => todo.completed)
};

export default {
    name: 'TodoApp',

    components: {
        TodoItem,
        TodoFooter
    },
    
    data: () => ({
        todos: JSON.parse(localStorage.getItem('todoAppData') || '[]'),
        visibility: 'all',
        beforeEditCancel: null,
        editedTodo: null
    }),

    watch: {
        todos: {
            handler(todos) {
                localStorage.setItem('todoAppData', JSON.stringify(todos))
            },
            deep: true
        }
    },

    mounted() {
        window.addEventListener('hashchange', this.onHashChange)
        this.onHashChange()
    },

    computed: {
        remaining() {
            return this.todos.length;
        },
        filteredTodos() {
            return filters[this.visibility](this.todos);
        }
    },

    methods: {
        onHashChange() {
            var visibility = window.location.hash.replace(/#\/?/, '');
            if (filters[visibility]) {
                this.visibility = visibility;
            } else {
                window.location.hash = '';
                this.visibility = 'all';
            }
        },
        toggleAll(e) {
            this.todos.forEach((todo) => (todo.completed = e.target.checked))
        },
        removeCompleted() {
            this.todos = filters.active(this.todos)
        },
        addTodo(event) {
            const value = event.target.value.trim();

            if (!value) {
                return;
            }

            this.todos.push({
                id: Date.now(),
                title: value,
                isCompleted: false
            });

            event.target.value = ''
        },
        removeTodo(todo) {
            this.todos.splice(this.todos.indexOf(todo), 1);
        },
        editTodo(todo) {
            this.beforeEditCancel = todo.title;
            this.editedTodo = todo;
        },
        cancelEdit(todo) {
            this.editedTodo = null
            todo.title = this.beforeEditCancel
        },
        doneEdit(todo) {
            this.editedTodo = null;
            todo.title = todo.title.trim();
            
            if (!todo.title) {
                this.removeTodo(todo);
            }
        }
    }
}
</script>

<template>
    <section class="todoapp">
        <header class="header">
            <h1>Todos</h1>
            <input autofocus class="new-todo" placeholder="What needs to be done?" @keyup.enter="addTodo">
        </header>
        <section class="main" v-show="todos.length">
            <input class="toggle-all" id="toggle-all" type="checkbox" :checked="remaining === 0" @change="toggleAll">
            <label for="toggle-all">Mark all as complete</label>
            <ul class="todo-list">
                <TodoItem 
                    :filteredTodos="filteredTodos"
                    :editedTodo="editedTodo"
                    @editTodo="editTodo"
                    @removeTodo="removeTodo"
                    @cancelEdit="cancelEdit"
                    @doneEdit="doneEdit"
                />
            </ul>
        </section>
        <TodoFooter
            :remaining="remaining"
            :visibility="visibility"
            :todos="todos"
            @removeCompleted="removeCompleted"
        />
    </section>
</template>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
