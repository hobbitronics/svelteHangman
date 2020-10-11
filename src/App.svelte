<head>
	<link href="https://unpkg.com/material-components-web@latest/dist/material-components-web.min.css" rel="stylesheet">
</head>

<script src="https://unpkg.com/material-components-web@latest/dist/material-components-web.min.js">
import Inputs from './inputs.svelte'
let game = [];
let won;
let word;
let wordArr = [];
let text;
let correct;
let guesses = [];
let graphic = ["", "___","|  O","| -|-","|  /|","|___"]
let hangman = [];
$: chances = 6 - guesses.length;
$: gameOver = `${wordArr.join('')} was the word. Game Over, Click Restart to play again.`

let wordList = [
    'apple', 'answer', 'anchor', 'bananna', 'berry', 'boat', 'boot', 'cape', 'cap', 'case', 'chip', 'cherry', 'cone', 'dark', 'deep', 'donkey', 'eight', 'ever', 'elephant', 'fire', 'food', 'good', 'great', 'hour', 'ink', 'jar', 'jump', 'juggle', 'kick', 'kangaroo', 'leopard', 'moon', 'night', 'opera', 'people', 'quiet', 'right', 'scissor', 'temple', 'unicorn', 'victorious', 'wink', 'xylophone', 'yelp', 'zebra'  //list game chooses from
];

const getList = (e) => {
wordList = e.detail.map(el => el.word);
start();
}

const randWord = () => wordList[Math.floor(Math.random()*wordList.length)];

const start = () => {
	won = false;
	word = randWord();
	hangman = [];
	guesses = [];
	correct = undefined;
	text = '';
	wordArr = word.split('');   //turns the string into an array
	game = wordArr.map(e => e = '_')
	}

const checkGuess = letter => wordArr.includes(letter) ? play(letter) : wrong(letter);

const wrong = letter => {
	hangman = [...hangman, graphic[guesses.length]]
	guesses = [...guesses, letter];
	correct = false;
}

const play = letter => {
	wordArr.forEach((el, index) => el === letter ? game[index] = letter : null)
	compareArrays()
	correct = true;
}

const compareArrays = () => JSON.stringify(game) === JSON.stringify(wordArr) ? won = true : won =false;

start();

</script>

<main>
	<h1>Welcome to Hangman</h1>
	<Inputs on:getlist={getList} />

	{#if !won && chances > 0}
		<h3>You have {chances} guesses.</h3>
		<span>Your guess:</span>
		<input bind:value={text} on:keydown={(event) => {if (event.key === 'Enter') checkGuess(text.toLowerCase())} } >
		<button on:click={() => checkGuess(text.toLowerCase())} class="mdc-button mdc-button--outlined">
			<div class="mdc-button__ripple"></div>
			<span class="mdc-button__label">guess</span>
		</button>
	{:else if !chances}
		<h2>{gameOver}</h2>
	{/if}

	<span>Wrong guesses: {guesses}</span>
	{#if won}	
		<p class="congrats">'You won! play again? Click reset to play'</p>
	{:else if correct === undefined}
			<p>New Game</p>
	{:else if correct}
		<p>'You got a letter right!'</p>
	{:else if !correct}
			<p>'Sorry, you guessed wrong.'</p>
	{/if}
	<div id="game">{game.join(' ')}</div>

	<button on:click={() => start()} class="mdc-button mdc-button--outlined">
        <div class="mdc-button__ripple"></div>
        <span class="mdc-button__label">Reset</span>
	</button>
	
	{#each hangman as man}
		<p class="man">{man}</p>
	{/each}
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 2em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}

	#game {
        font-size: 2.5em;
      }
      span, p {
        font-size: 1.5em;
        padding-right: 1px;
		margin: 0;
      }
      input {
        font-size: 2em;
        width: 150px;
      }

	  .congrats {
		  font-size: 1.5em;
		  font-weight: bold;
		  color: rgb(255, 0, 225);
	  }

	  .man {
		  text-align: center;
	  }
</style>