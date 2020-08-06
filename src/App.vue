<template>
	<div id="app">
		<div class="round">Round {{this.round}}</div>

		<div class="game-board">
			<div class="board-top">
				<div class="tile tile-top-left"
					@click="clickHandler(0)"
					:class="{flash: this.greenClicked}"
				></div>

				<div class="tile tile-top-right"
					@click="clickHandler(1)"
					:class="{flash: this.redClicked}"
				></div>
			</div>
			<div class="center-plate">
				<div class="start-wrapper">
					<div class="start-btn" @click="startGame">
						<span>start</span>
					</div>
				</div>
			</div>
			<div class="board-bottom">
				<div class="tile tile-bottom-left"
					@click="clickHandler(2)"
					:class="{flash: this.blueClicked}"
				></div>
				
				<div class="tile tile-bottom-right"
					@click="clickHandler(3)"
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
	data() {
		return {
			round: 0,
			difficulty: 1500,
			panelActive: false,
			gameActive: false,
			gameOver: false,

			sequence: [],
			playerSequence: [],

			greenClicked: false,
			redClicked: false,
			blueClicked: false,
			violetClicked: false,



		}
	},
	methods: {
	
		startGame() {
			if (this.gameActive) this.gameActive = false

			this.panelActive = true
			this.gameActive = true
			
			this.newRound()

		},

		newRound() {
			this.round++
			this.increaseSequence()
			this.playSequence()
		},

		

		increaseSequence() {
			this.sequence.push(this.getRandomNumber())
		},

		playSequence() {
			this.panelActive = false
			let i = 0
			
			let forSequence = setInterval( () => {
				this.activateTile(this.sequence[i])

				i++
				if (i >= this.sequence.length) {
					clearInterval(forSequence)
					this.panelActive = true
				}
			}, this.difficulty)
		},

		clickHandler(tileNum) {
			if (!this.panelActive) return
			this.activateTile(tileNum)
			this.checkLose(tileNum)
		},

		checkLose() {
			this.newRound()
		},

		activateTile(tileNum) {
			if (tileNum === 0) this.activateGreen()
			else if (tileNum === 1) this.activateRed()
			else if (tileNum === 2) this.activateBlue()
			else if (tileNum === 3) this.activateViolet()
		},

		activateGreen() {
			this.greenClicked = true
			setTimeout( () => {
				this.greenClicked = false
			}, 500)
		},

		activateRed() {
			this.redClicked = true
			setTimeout( () => {
				this.redClicked = false
			}, 500)
		},

		activateBlue() {
			this.blueClicked = true
			setTimeout( () => {
				this.blueClicked = false
			}, 500)
		}, 

		activateViolet() {
			this.violetClicked = true
			setTimeout( () => {
				this.violetClicked = false
			}, 500)
		},

		getRandomNumber() {
			return Math.floor((Math.random()*4))
		},

		delay(ms) {
  			return new Promise((resolve) => setTimeout(resolve, ms));
		}
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

	.start-wrapper {
		display: flex;
		justify-content: center;
		align-items: center;
		height: 100%;
		width: 100%;

		.start-btn {
			cursor: pointer;
			background: rgb(25, 65, 61);
			width: 4rem;
			height: 2rem;
			border-radius: 10px;
			color: #bdbdbd;
			font-size: 1.1rem;
			user-select: none;
			display: flex;
			justify-content: center;
			align-items: center;
		}
	}

}

.tile {
	display: inline-block;
	width: 200px;
	height: 200px;
	margin: 0 1px;
	cursor: pointer;

	&:hover {
		border: 1px solid rgb(2, 163, 163);
	}
}

.tile-top-left {
	background: rgb(0, 112, 0);
	border-top-left-radius: 100%;
}
.tile-top-right {
	background: rgb(167, 1, 1);
	border-top-right-radius: 100%;
}
.tile-bottom-left {
	background: rgb(0, 0, 151);
	border-bottom-left-radius: 100%;
}
.tile-bottom-right {
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
