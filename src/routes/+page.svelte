<script lang="ts">
	type Subtask = {
		id: number;
		name: string;
		status: boolean;
	};
	type Task = {
		id: number;
		name: string;
		subtasks?: Subtask[];
	};

	const tasks: Task[] = $state([
		{ id: 1, name: 'gowno', subtasks: [{ id: 121, name: 'dupaaa', status: false }] },
		{ id: 2, name: 'gownoda', subtasks: [{ id: 124, name: 'dupa', status: false }] },
		{ id: 3, name: 'gownodadadadadadadada', subtasks: [] }
	]);

	let taskName = $state('New task');
	let subtaskInputs = $state<{ [key: number]: string }>({});

	function addTask() {
		tasks.push({ id: Date.now(), name: taskName });
		taskName = '';
	}

	function addSubtask(taskId: number) {
		const name = subtaskInputs[taskId];
		if (!name) return;

		const foundTask = tasks.find((task) => task.id === taskId);
		if (foundTask) {
			if (!foundTask.subtasks) foundTask.subtasks = [];
			foundTask.subtasks.push({
				id: Date.now(), // unique id
				name,
				status: false
			});
			subtaskInputs[taskId] = ''; // clear input
		}
	}
</script>

<div class="app">
	<div class="tasks">
		{#each tasks as task}
			<div class="task">
				<div class="task-details">
					<input type="checkbox" />
					<div class="task-text">{task.name}</div>
				</div>
				{#if task.subtasks != undefined}
					<div class="subtasks">
						{#each task.subtasks as subtask}
							<div class="subtask">
								<input type="checkbox" />
								<div class="subtask-text">{subtask.name}</div>
							</div>
						{/each}
					</div>
				{/if}
				<div class="new-task">
					<input type="text" bind:value={subtaskInputs[task.id]} />
					<button onclick={() => addSubtask(task.id)}>-></button>
				</div>
			</div>
		{/each}
		<div class="new-task">
			<input type="text" bind:value={taskName} />
			<button onclick={() => addTask()}>-></button>
		</div>
	</div>
</div>

<style lang="scss">
	.app {
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
		margin-top: 24px;
		.tasks {
			.task {
				border: 1px gray solid;
				border-radius: 6px;
				margin-bottom: 12px;
				.task-details {
					display: flex;
					padding: 4px;
					width: 300px;
					.task-text {
						margin-left: 8px;
					}
				}
				.subtasks {
					.subtask {
						display: flex;
						padding: 4px;
						width: 280px;
						padding-left: 24px;
						.subtask-text {
							margin-left: 8px;
						}
					}
				}
			}
		}
	}
</style>
