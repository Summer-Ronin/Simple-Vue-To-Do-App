<template>
	<img :src="logoURL" width="200" height="200" />
	<h1>{{ title }}</h1>

	<h2>Add a new task</h2>
	<div>
		<!-- This should be a form tho-->
		<span
			>You have {{ allTasks }} {{ allTasks > 1 ? "tasks" : "task" }} at
			the moment</span
		>
		<br />

		<input
			type="text"
			v-model="newTask"
			placeholder="Enter new task here"
		/>

		<button @click="addTask" :disabled="newTask.length < 1">
			Add Task
		</button>
	</div>

	<!--Two way binding-->
	<div v-if="newTask.length > 0">
		<h3>New task preview</h3>
		<p>{{ newTask }}</p>
	</div>

	<ul>
		<li
			v-for="(task, index) in tasks"
			:key="task.id"
			@click="finishTask(task)"
			:class="{ strike: task.finished }"
		>
			{{ index + 1 }}. {{ task.name }} - {{ task.finished }}

			<div v-if="task.finished" @click="removeTask(task.id)">
				<button>Delete task</button>
			</div>
		</li>
	</ul>
</template>

<style>
	.strike {
		text-decoration: line-through;
	}
</style>

<script>
	export default {
		data() {
			return {
				title: "My To Do App",
				newTask: "",
				tasks: [
					{ id: 1, name: "Task number 1", finished: false },
					{ id: 2, name: "Task number 2", finished: false },
					{ id: 3, name: "Task number 3", finished: false },
				],
				logoURL:
					"https://upload.wikimedia.org/wikipedia/commons/f/f1/Vue.png",
			};
		},

		methods: {
			addTask() {
				// check if newTask is filled
				if (this.newTask.length < 1) {
					return;
				} else {
					this.tasks.push({
						id: this.tasks.length + 1,
						name: this.newTask,
						finished: false,
					});
				}

				// Clear out newTask to store new one
				this.newTask = "";
			},

			finishTask(task) {
				task.finished = !task.finished;
			},

			removeTask(taskID) {
                // Using filter is a choice too but not a good way to understand
                // It uses Array.filter to return elements not matching a value.
				this.tasks = this.tasks.filter((task) => {
					return task.id != taskID;
				});
				console.log(this.tasks.length);
			},
		},

		computed: {
			allTasks() {
				return this.tasks.length;
			},
			// latest() {
			// 	return [...this.tasks].reverse();
			// },
		},
	};
</script>
