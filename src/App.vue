<template>
	<div id="app">
		<div class="round">Round {{this.round}}</div>

		<div class="game-board">
			<div class="board-top">
				<div class="button button-top-left"
					@click="green()"
					:class="{flash: this.greenClicked}"
				></div>

				<div class="button button-top-right"
					@click="red()"
					:class="{flash: this.redClicked}"
				></div>
			</div>
			<div class="center-plate">
				<div class="toggle-start-wrapper">
					<label>Start/Stop 
						<input type="checkbox" 
							v-model="gameON"
						>
					</label>
				</div>
			</div>
			<div class="board-bottom">
				<div class="button button-bottom-left"
					@click="blue()"
					:class="{flash: this.blueClicked}"
				></div>
				
				<div class="button button-bottom-right"
					@click="violet()"
					:class="{flash: this.violetClicked}"
				></div>
			</div>
		</div>
		<div class="game-control">
			<div class="difficulty">
				<p>Difficulty</p>
				<label class="toggle easy">
					<input type="radio" name="difficulty" value="1500"
						v-model="difficulty"
					>
				</label>
				<label class="toggle normal">
					<input type="radio" name="difficulty" value="1000"
						v-model="difficulty"
					>
				</label>
				<label class="toggle hard">
					<input type="radio" name="difficulty" value="400"
						v-model="difficulty"
					>
				</label>
			</div>
		</div>
	</div>
</template>

<script>


export default {
	name: 'App',
	components: {},
	computed: {

	},
	data() {
		return {
			round: 0,
			difficulty: 1500,
			gameON: false,

			greenClicked: false,
			redClicked: false,
			blueClicked: false,
			violetClicked: false,

			colorsDict: {
				0: 'green',
				1: 'red',
				2: 'blue',
				3: 'violet'
			},


			sequence: [],
			playerSequence: [],
		}
	},
	methods: {
		green() {
			if (!this.gameON) return
			this.greenClicked = true

			

			setTimeout( f => {
				this.greenClicked = false
			}, 500)
		},
		red() {
			if (!this.gameON) return
			this.redClicked = true




			setTimeout( f => {
				this.redClicked = false
			}, 500)
		},
		blue() {
			if (!this.gameON) return
			this.blueClicked = true



			setTimeout( f => {
				this.blueClicked = false
			}, 500)
		},
		violet() {
			if (!this.gameON) return
			this.violetClicked = true



			setTimeout( f => {
				this.violetClicked = false
			}, 500)
		},

	}
}
</script>

<style lang="scss">
*,*:before,*:after{-moz-box-sizing: border-box;-webkit-box-sizing: border-box;box-sizing: border-box;}

body {
	background: black;
}

#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #bdbdbd;
	margin-top: 100px;
}

.round {
	font-size: 1.7em;
	margin-bottom: 1rem;
}

.game-board {
	display: inline-block;
	position: relative;
}

.board-bottom {
	margin-top: -2px;
}

.center-plate {
	position: absolute;
	display: inline-block;
	width: 100px;
	height: 100px;
	background: black;
	border-radius: 50%;
	top: 150px;
	left: 152px;
	z-index: 100;

	.toggle-start-wrapper {
		font-size: .8em;
		height: 100%;
		width: 100%;
		display: flex;
		justify-content: center;
		align-items: center;

		label {
			user-select: none;
		}
	}
}

.button {
	display: inline-block;
	width: 200px;
	height: 200px;
	margin: 0 1px;
	cursor: pointer;

	&:hover {
		border: 1px solid rgb(2, 163, 163);
	}
}

.button-top-left {
	background: rgb(0, 112, 0);
	border-top-left-radius: 100%;
}
.button-top-right {
	background: rgb(167, 1, 1);
	border-top-right-radius: 100%;
}
.button-bottom-left {
	background: rgb(0, 0, 151);
	border-bottom-left-radius: 100%;
}
.button-bottom-right {
	background: rgb(87, 0, 128);
	border-bottom-right-radius: 100%;
}

.flash {
	filter: brightness(1.5);
}

.game-control {
	width: 10em;
	height: 3.5em;
	margin: 0 auto;
	margin-top: 2em;

	.difficulty {

		.toggle {
			padding: .6em 1em;
			border-radius: 50%;
			display: inline-block;
			cursor: pointer;
		}

		.easy {
			background: rgb(2, 73, 2);
		}
		.normal {
			background: rgb(2, 2, 99);
		}
		.hard {
			background: rgb(97, 1, 1);
		}
	}

}

</style>
