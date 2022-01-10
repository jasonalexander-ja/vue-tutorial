<template>
    <header>
        <h1>Yask Tacker</h1>
        <Button
            v-show="homePage"
            :text="showAddTask ? 'Close' : 'Add Task'"
            :color="showAddTask ? 'red' : 'green'"
            @btn-click="$emit('toggle-add-task')"
        />
    </header>
</template>
<script>
import Button from './Button';
export default {
    name: 'Header',
    props: {
        title: {
            type: String,
            default: 'Hello world',
        },
        showAddTask: Boolean
    },
    components: {
        Button
    },
    emits: ['toggle-add-task'],
    computed: {
        homePage() {
            if(this.$route.path === "/") {
                return true;
            } else return false;
        }
    }
}
</script>
<style scoped>
    header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
    }
</style>