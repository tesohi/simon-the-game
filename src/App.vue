<template>
	<div id="app">
		<div class="game-over" v-if="this.gameIsOver">
			<span>The Game is Over</span>
		</div>
		<div class="game-in-progress" v-if="this.gameActive">
			<span>The Game in Progress</span>
		</div>

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
				<div class="control-wrapper">
					<div v-if="!this.gameActive" class="btn btn-start" @click="startGame">
						<span>start</span>
					</div>
					<div v-if="this.gameActive" class="btn btn-stop" @click="stopGame">
						<span>stop</span>
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
		<div class="game-settings">
			<div class="difficulty">
				<p>Difficulty</p>
				<label class="toggle easy">
					<input type="radio" name="difficulty" value="1500"
						v-model="difficulty"
						:disabled="this.gameActive && this.difficulty != 1500"
					>
				</label>
				<label class="toggle normal">
					<input type="radio" name="difficulty" value="1000"
						v-model="difficulty"
						:disabled="this.gameActive && this.difficulty != 1000"
					>
				</label>
				<label class="toggle hard">
					<input type="radio" name="difficulty" value="400"
						v-model="difficulty"
						:disabled="this.gameActive && this.difficulty != 400"
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
			difficulty: 1000,
			panelActive: false,
			gameActive: false,
			gameIsOver: false,

			sequenceChecked: false,

			sequence: [],
			sequenceToCheck: [],

			greenClicked: false,
			redClicked: false,
			blueClicked: false,
			violetClicked: false,



		}
	},
	methods: {
	
		startGame() {
			if (this.gameActive) return

			this.gameIsOver = false
			this.panelActive = true
			this.gameActive = true
			
			this.newRound()

		},

		stopGame() {
			this.sequence = []
			this.sequenceToCheck = []
			this.gameActive = false
			this.panelActive = false
			this.sequenceChecked = false
			this.round = 0
		},

		setGameOver() {
			this.sequence = []
			this.sequenceToCheck = []
			this.gameActive = false
			this.panelActive = false
			this.sequenceChecked = false
			this.round = 0
			this.gameIsOver = true
		},

		newRound() {
			this.sequenceChecked = false
			this.round++
			this.increaseSequence()
			this.fillSequenceToCheck()
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

		fillSequenceToCheck() {
			this.sequenceToCheck = this.sequence.slice(0)
		},

		clickHandler(tileNum) {
			if (!this.panelActive) return
			this.activateTile(tileNum)

			let tileIsCorrect = this.checkCorrectTile(tileNum)

			if (this.sequenceToCheck.length === 0) {
				this.sequenceChecked = true
			}

			if (tileIsCorrect && this.sequenceChecked) {
				setTimeout ( () => {
					this.newRound()
				}, 1500)
			}

			if (!tileIsCorrect) {
				this.setGameOver()
			}

		},

		checkCorrectTile(tileNum) {
			let desiredResponse = this.sequenceToCheck.shift()

			if (tileNum === desiredResponse) {
				return true
			} 
			else {
				return false
			}			
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
			}, this.getTimeFlashing())
		},

		activateRed() {
			this.redClicked = true
			setTimeout( () => {
				this.redClicked = false
			}, this.getTimeFlashing())
		},

		activateBlue() {
			this.blueClicked = true
			setTimeout( () => {
				this.blueClicked = false
			}, this.getTimeFlashing())
		}, 

		activateViolet() {
			this.violetClicked = true
			setTimeout( () => {
				this.violetClicked = false
			}, this.getTimeFlashing())
		},

		getTimeFlashing() {
			return this.difficulty / 2
		},

		getRandomNumber() {
			return Math.floor((Math.random()*4))
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
	position: relative;
}

.game-over {
	position: absolute;
	top: -50px;
	width: 100%;
	display: flex;
	justify-content: center;
	color: rgb(209, 0, 0);
}

.game-in-progress {
	position: absolute;
	top: -50px;
	width: 100%;
	display: flex;
	justify-content: center;
	color: rgb(5, 119, 5);
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

	.control-wrapper {
		display: flex;
		justify-content: center;
		align-items: center;
		height: 100%;
		width: 100%;

		.btn {
			cursor: pointer;
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

		.btn-start {
			background: rgb(25, 65, 61);
		}

		.btn-stop {
			background: rgb(65, 27, 38);
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

.game-settings {
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
