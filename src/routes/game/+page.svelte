<script lang="ts">
	import { fade } from 'svelte/transition';

	function getRandomInt(max: number) {
		return Math.floor(Math.random() * max);
	}

	const reset = async () => {
		table = [
			[0, 0, 0],
			[0, 0, 0],
			[0, 0, 0]
		];
		result = null;
	};

	let result: 'win' | 'loss' | null = null;

	type Table = [[number, number, number], [number, number, number], [number, number, number]];

	let table: Table = [
		[0, 0, 0],
		[0, 0, 0],
		[0, 0, 0]
	];

	let isPlayerTure: boolean = true;

	const changeValue = (line_index: number, row_index: number) => {
		if (table[line_index][row_index] === 0 && isPlayerTure) {
			table[line_index][row_index] = 1;
			isPlayerTure = false;
		}
	};

	$: if (
		(1 === table[0][0] && table[0][0] === table[1][0] && table[1][0] === table[2][0]) ||
		(1 === table[0][1] && table[0][1] === table[1][1] && table[1][1] === table[2][1]) ||
		(1 === table[0][2] && table[0][2] === table[1][2] && table[1][2] === table[2][2]) ||
		(1 === table[0][0] && table[0][0] === table[1][1] && table[1][1] === table[2][2]) ||
		(1 === table[0][2] && table[0][2] === table[1][1] && table[1][1] === table[2][0]) ||
		(1 === table[0][0] && table[0][0] === table[0][1] && table[0][1] === table[0][2]) ||
		(1 === table[1][0] && table[1][0] === table[1][1] && table[1][1] === table[1][2]) ||
		(1 === table[2][0] && table[2][0] === table[2][1] && table[2][1] === table[2][2])
	) {
		result = 'win';
		isPlayerTure = true;
	}

	$: if (
		(2 === table[0][0] && table[0][0] === table[1][0] && table[1][0] === table[2][0]) ||
		(2 === table[0][1] && table[0][1] === table[1][1] && table[1][1] === table[2][1]) ||
		(2 === table[0][2] && table[0][2] === table[1][2] && table[1][2] === table[2][2]) ||
		(2 === table[0][0] && table[0][0] === table[1][1] && table[1][1] === table[2][2]) ||
		(2 === table[0][2] && table[0][2] === table[1][1] && table[1][1] === table[2][0]) ||
		(2 === table[0][0] && table[0][0] === table[0][1] && table[0][1] === table[0][2]) ||
		(2 === table[1][0] && table[1][0] === table[1][1] && table[1][1] === table[1][2]) ||
		(2 === table[2][0] && table[2][0] === table[2][1] && table[2][1] === table[2][2])
	) {
		result = 'loss';
		isPlayerTure = true;
	}

	$: {
		let i = 0;

		while (!isPlayerTure && i < 100) {
			let line_index = getRandomInt(3);
			let row_index = getRandomInt(3);

			if (table[line_index][row_index] === 0) {
				table[line_index][row_index] = 2;

				isPlayerTure = true;
			}

			i++;
		}
	}
</script>

<div class="gap">
	<div class="end">
		{#if result === 'win'}
			<h1 transition:fade={{ duration: 150 }}>You Win!</h1>
		{:else if result === 'loss'}
			<h1 transition:fade={{ duration: 150 }}>You Loss!</h1>
		{/if}
		<div>
			<a href="/game" on:click={() => reset()}>Return</a>
			<a href="https://github.com/seba9989/Dot-And-Cross">GitHub</a>
		</div>
	</div>

	<div class="grid">
		{#each table as line, line_index (line_index)}
			{#each line as value, row_index (row_index)}
				<button on:click={() => changeValue(line_index, row_index)}>
					{#if value === 2}
						<div class="O" in:fade={{ duration: 150 }} />
					{:else if value === 1}
						<div class="X" in:fade={{ duration: 150 }} />
					{:else}
						<div />
					{/if}
				</button>
			{/each}
		{/each}
	</div>
</div>

<style lang="scss">
	.gap {
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: column;
		justify-content: space-around;
		align-items: center;
	}

	.grid {
		display: grid;
		grid-template-columns: repeat(3, 200px);
		grid-template-rows: repeat(3, 200px);
		gap: 10px;
		background-color: #ddd;

		button {
			background: #222;
			border: none;
			width: 100%;
			height: 100%;
			display: flex;
			justify-content: center;
			align-items: center;

			.O {
				background: #222;
				width: 75%;
				aspect-ratio: 1 / 1;
				border-radius: 100%;
				border: solid;
				border-color: #ddd;
				border-width: 10px;
			}

			.X {
				aspect-ratio: 1 / 1;
				width: 75%;
				position: relative;

				&::after {
					content: '';
					position: absolute;
					bottom: 50%;
					left: 0%;
					background: #ddd;
					width: 100%;
					height: 10px;
					rotate: 45deg;
				}

				&::before {
					content: '';
					position: absolute;
					bottom: 50%;
					left: 0%;
					background: #ddd;
					width: 100%;
					height: 10px;
					rotate: -45deg;
				}
			}
		}
	}

	.end {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		color: #ddd;

		h1 {
			font-size: 4rem;
		}

		div {
			display: flex;
			flex-direction: row;
			justify-content: center;
			align-items: center;
			gap: 50px;

			font-size: 2rem;

			a {
				position: relative;
				color: #aaa;
				text-decoration: none;

				&::before {
					content: '';

					position: absolute;
					width: 0;
					height: 2px;
					bottom: -2px;

					background: #aaa;

					transition: all 250ms;
				}

				&:hover::before {
					width: 100%;
				}
			}
		}
	}
</style>
