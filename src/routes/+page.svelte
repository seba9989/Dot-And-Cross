<div>
	<h1>Kółko i Krzyżyk</h1>

	<div>
		<a href="game">Start</a>
		<a href="">GitHub</a>
	</div>
</div>

<style lang="scss">
	div {
		display: flex;
		flex-direction: column;

		h1 {
			color: #ddd;
			font-size: 4rem;
		}

		div {
			display: flex;
			flex-direction: row;
			justify-content: space-around;
			align-items: center;

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
