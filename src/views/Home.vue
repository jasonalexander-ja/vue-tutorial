<template>
    <AddTask
        v-if="showAddTask"
        @add-task="addTask"
    />
    <Tasks 
        @delete-task="deleteTask" 
        @toggle-reminder="toggleReminder"
        :tasks="tasks"
    />
</template>
<script>
import Tasks from '../components/Tasks';
import AddTask from '../components/AddTask';

export default {
	name: "Home",
    components: {
        Tasks,
        AddTask
    },
    props: {
        showAddTask: Boolean
    },
	data() {
		return {
			tasks: [],
        }
    },
	async created() {
		this.tasks = await this.fetchTasks();
	},
	methods: {
		async deleteTask(id) {
			if(confirm("Are you sure?")) {
				const res = await fetch(`api/tasks/${id}`, {
					method: 'DELETE'
				});
				res.status === 200 ? (
					this.tasks = this.tasks.filter(t => t.id !== id)
				) : alert("Error deleting tasks.")

				
			}
		},
		async toggleReminder(id) {
			const taskToToggle = await this.fetchTask(id);
			const updTask = {...taskToToggle, reminder: !taskToToggle.reminder};

			const res = await fetch(`api/tasks/${id}`, {
				method: 'PUT',
				headers: {
					'Content-type': 'application/json'
				},
				body: JSON.stringify(updTask)
			});

			const data = await res.json();

			const taskIndex = this.tasks.findIndex(t => t.id === id);
			const oldTask = this.tasks[taskIndex];
			const newTask = {...oldTask, reminder: data.reminder}
			this.tasks[taskIndex] = newTask;
		},
		async addTask(task) {
			const res = await fetch('api/tasks', {
				method: 'POST',
				headers: {
					'Content-type': 'application/json',
				},
				body: JSON.stringify(task)
			});

			const data = await res.json();

			this.tasks = [...this.tasks, data]
		},
		async fetchTasks() {
			const res = await fetch('api/tasks');
			const tasks = await res.json();
			return tasks;
		},
		async fetchTask(id) {
			const res = await fetch(`api/tasks/${id}`);
			const tasks = await res.json();
			return tasks;
		}
	}
}
</script>