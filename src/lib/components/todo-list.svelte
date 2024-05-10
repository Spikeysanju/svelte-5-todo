<script lang="ts">
	import TaskItem from './task-item.svelte';
	import type { Task } from '$lib';

	let value = $state(''); // Input value
	let tasks = $state<Task[]>([]);
	let tags = $state.frozen(['Work', 'Personal', 'Important']); // Immutable list of tags

	// Taking snapshots of the state for undo/redo functionality
	let taskSnapshot: Task[][] = [];

	let totalTasks = $derived.by(() => tasks.length);
	let completedTasks = $derived.by(() => tasks.filter((task) => task.completed).length);

	// Filtering tasks using $derived.by
	let filteredTasks = $derived.by(() => {
		return tasks.filter((task) => !task.completed);
	});

	// Filtering completed tasks using $derived.by
	let filteredCompleteTasks = $derived.by(() => {
		return tasks.filter((task) => task.completed);
	});

	// Save tasks to local storage whenever they change
	$effect(() => {
		// get the tasks from local storage
		const savedTasks = localStorage.getItem('tasks');
		if (savedTasks) {
			tasks = JSON.parse(savedTasks);
		}

		// save the tasks to local storage whenever they change
		$effect(() => {
			localStorage.setItem('tasks', JSON.stringify(tasks));
		});
	});

	// Function to take a snapshot of the current tasks state
	function takeSnapshot() {
		taskSnapshot.push($state.snapshot(tasks));
	}

	// Undo function to restore the previous tasks state
	function undo() {
		if (taskSnapshot.length > 0) {
			tasks = taskSnapshot.pop()!;
		}
	}

	// Add a new task
	function addTask(description: string, tag: string) {
		takeSnapshot();
		tasks = [...tasks, { description, tag, completed: false }];

		// Clear the input field
		value = '';
	}

	// Delete all tasks
	function deleteAllTasks() {
		takeSnapshot();
		tasks = [];
	}

	// Function to toggle task completion status
	function toggleTaskCompletion(task: Task) {
		const index = tasks.indexOf(task);
		if (index > -1) {
			tasks[index].completed = !tasks[index].completed;
		}
	}

	// Using $effect.pre to autoscroll the task list before the DOM updates
	$effect.pre(() => {
		const taskListElement = document.getElementById('taskList') as HTMLElement;
		if (taskListElement) {
			taskListElement.scrollTop = taskListElement.scrollHeight;
		}
	});

	// Debugging tasks state with $inspect
	$inspect(tasks);
</script>

<div>
	<h1>Todo List</h1>
	<p>Total tasks: {totalTasks} | Completed tasks: {completedTasks}</p>

	<input
		type="text"
		bind:value
		placeholder="Enter task"
		onkeydown={(e) =>
			e.key === 'Enter' && addTask(value, tags[Math.floor(Math.random() * tags.length)])}
	/>

	<ul id="taskList">
		{#each filteredTasks as task}
			<TaskItem {task} {toggleTaskCompletion} />
		{/each}
	</ul>

	<h2>Completed Tasks</h2>
	<button onclick={undo}>Undo</button>
	<button onclick={deleteAllTasks}>Delete All</button>

	<ul>
		{#each filteredCompleteTasks as task}
			<TaskItem {task} {toggleTaskCompletion} />
		{/each}
	</ul>
</div>
