<script setup>
import { defineProps, defineEmits } from 'vue';


const props = defineProps({
    todo: {
        type: Object,
        required: true
    }
});

const emit = defineEmits(['remove']);
const removeTodo = (todo) => {
    emit('remove', todo);
};
</script>

<style scoped></style>

<template>

    <div :class="`todo-item ${todo.done && 'done'} ${todo.priority}`">
        <div :class="`item-icon ${todo.category} ${todo.done && 'done'}`"></div>
        <div class="todo-info">
            <label>
                <input type="checkbox" v-model="todo.done">
                <span :class="`bubble ${todo.priority}`"></span>
            </label>
            <div class="todo-content">
                <input type="text" v-model="todo.content" />
            </div>
            <div class="actions">
                <button class="delete" @click="removeTodo(todo)">Excluir</button>
            </div>
        </div>
    </div>




</template>