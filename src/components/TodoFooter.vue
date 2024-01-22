<script>
export default {
    name: 'TodoFooter',

    props: [],
    emits: [],

    data: () => ({}),

    watch: {},

    mounted() {

    },

    computed: {

    },

    methods: {
    }
}
</script>

<template>
    <footer class="footer" v-show="todos.length">
        <span class="todo-count">
            <strong>{{ remaining }}</strong>
            <span>{{ remaining === 1 ? ' item' : ' items' }} left</span>
        </span>
        <ul class="filters">
            <li>
                <a href="#/all" :class="{ selected: visibility === 'all' }">All</a>
            </li>
            <li>
                <a href="#/active" :class="{ selected: visibility === 'active' }">Active</a>
            </li>
            <li>
                <a href="#/completed" :class="{ selected: visibility === 'completed' }">Completed</a>
            </li>
        </ul>
        <button class="clear-completed" @click="removeCompleted" v-show="todos.length > remaining">
            Clear completed todos
        </button>
    </footer>
</template>