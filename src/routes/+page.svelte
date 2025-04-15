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
		{ id: 2, name: 'gownoda', subtasks: [{ id: 121, name: 'dupa', status: false }] },
		{ id: 3, name: 'gownodadadadadadadada' }
	]);

	let taskName = $state('New task');

	function addTask() {
		tasks.push({ id: 4, name: taskName });
		taskName = '';
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
						<div class="new-task">
							<input type="text" />
						</div>
					</div>
				{/if}
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
