<script lang="ts">
	import type { Task } from '$lib';

	// Declare props and bindable event for task completion
	const {
		task = { description: '', tag: '', completed: false },
		toggleTaskCompletion = () => {}
	}: {
		task?: Task;
		toggleTaskCompletion: (task: Task) => void;
	} = $props();

	// Function to toggle completion status
	function handleToggleCompletion() {
		toggleTaskCompletion(task);
	}
</script>

<li>
	<input type="checkbox" checked={task.completed} onclick={handleToggleCompletion} />
	{task.description} [{task.tag}]
</li>
