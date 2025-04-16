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

	let taskName = $state('');
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
					<input type="checkbox" class="circle-checkbox" />
					<div class="task-text">{task.name}</div>
				</div>
				{#if task.subtasks != undefined}
					<div class="subtasks">
						{#each task.subtasks as subtask}
							<div class="subtask">
								<input type="checkbox" class="circle-checkbox" />
								<div class="subtask-text">{subtask.name}</div>
							</div>
						{/each}
					</div>
				{/if}
				<div class="new-task">
					<div class="fake-circle-checkbox"></div>
					<input
						type="text"
						class="task-input"
						placeholder="New task..."
						bind:value={subtaskInputs[task.id]}
					/>
					<button onclick={() => addSubtask(task.id)}>+</button>
				</div>
			</div>
		{/each}
		<div class="new-task">
			<div class="fake-circle-checkbox"></div>
			<input type="text" class="task-input" placeholder="New task..." bind:value={taskName} />
			<button onclick={() => addTask()}>+</button>
		</div>
	</div>
</div>

<style lang="scss">
	@import url('https://fonts.googleapis.com/css2?family=VT323&display=swap');

	:global(body) {
		margin: 0px;
		background-color: #7e1891;
		font-family: 'VT323', monospace;
		font-weight: 400;
		font-style: normal;
	}

	.circle-checkbox {
		width: 1.3em;
		height: 1.3em;
		background-color: white;
		border-radius: 50%;
		vertical-align: middle;
		border: 1px dotted #ddd;
		appearance: none;
		-webkit-appearance: none;
		outline: none;
		cursor: pointer;
		&:checked {
			background-color: #fcc737;
		}
	}

	.fake-circle-checkbox {
		width: 0.8em;
		height: 0.8em;
		background-color: white;
		border-radius: 50%;
		vertical-align: middle;
		border: 1px dotted #ddd;
		appearance: none;
		-webkit-appearance: none;
		outline: none;
	}

	.app {
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
		margin-top: 24px;
		color: white;
		.tasks {
			.task {
				margin-bottom: 12px;
				.task-details {
					display: flex;
					padding: 4px;
					width: 360px;
					height: 28px;
					background-color: #e73879;
					border-radius: 8px;
					align-items: center;
					.task-text {
						margin-left: 8px;
						font-size: 20px;
					}
				}
				.subtasks {
					.subtask {
						display: flex;
						padding: 4px;
						width: 340px;
						height: 28px;
						padding-left: 24px;
						background-color: #e73879;
						border-radius: 8px;
						margin-top: 2px;
						align-items: center;
						.subtask-text {
							margin-left: 8px;
							font-size: 20px;
						}
					}
				}
				.new-task {
					display: flex;
					padding: 4px;
					width: 340px;
					height: 28px;
					background-color: #e73879;
					border-radius: 8px;
					margin-top: 2px;
					padding-left: 24px;
					align-items: center;

					.task-input[type='text'] {
						margin-left: 8px;
						outline: none;
						border: none;
						font-size: 20px;
						font-family: 'VT323', monospace;
						font-weight: 400;
						font-style: normal;
						background-color: #e73879;
						padding: 4px;
						&::placeholder {
							color: #ffc8e6;
						}
					}
				}
			}
			.new-task {
				display: flex;
				padding: 4px;
				width: 360px;
				height: 28px;
				background-color: #e73879;
				border-radius: 8px;
				margin-top: 2px;
				align-items: center;
				.task-input[type='text'] {
					margin-left: 8px;
					outline: none;
					border: none;
					font-size: 20px;
					font-family: 'VT323', monospace;
					font-weight: 400;
					font-style: normal;
					background-color: #e73879;
					padding: 4px;
					&::placeholder {
						color: #ffc8e6;
					}
				}
			}
		}
	}
</style>
