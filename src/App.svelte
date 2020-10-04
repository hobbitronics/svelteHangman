<script>
import Inputs from './inputs.svelte'
let name = 'dude';
let game = [];
let word;
let wordArr = [];
let won;
let text;
let lastGuess;
let guesses = [];
let message = '';
const wonGame = 'You won! play again? Click reset to play';
$: chances = 6 - guesses.length;
$: gameOver = `${wordArr.join('')} was the word. Game Over, Click Restart to play again.`

let wordList = [
    'apple', 'answer', 'anchor', 'bananna', 'berry', 'boat', 'boot', 'cape', 'cap', 'case', 'chip', 'cherry', 'cone', 'dark', 'deep', 'donkey', 'eight', 'ever', 'elephant', 'fire', 'food', 'good', 'great', 'hour', 'ink', 'jar', 'jump', 'juggle', 'kick', 'kangaroo', 'leopard', 'moon', 'night', 'opera', 'people', 'quiet', 'right', 'scissor', 'temple', 'unicorn', 'victorious', 'wink', 'xylophone', 'yelp', 'zebra'  //list game chooses from
];

const getList = (e) => {
let wordData = e.detail;
wordList = wordData.map(el => el.word);
console.log(wordList);
start();
}

//chooses a random word from the list as a string
const random = () => {
return wordList[Math.floor(Math.random()*wordList.length)];
}
//resets changes and gets a random word from the list
const start = () => {
	// chances = 6;
	word = random();
	guesses = [];
	lastGuess = '';
	text = '';
	message = '';
	wordArr = word.split('');   //turns the string into an array
	game = [];
	for (let i = 0; i < wordArr.length; i++)     //creates/resets the gameboard for player to see how many blanks
		{
    		game[i] = '_';
		}
	}

//takes guess and checks if in array and adds to game
function play (arg) {
		lastGuess = arg.toLowerCase();
		if (wordArr.includes(lastGuess)) {
			wordArr.forEach((el, index) => {
				if (el === lastGuess){
					game[index] = lastGuess;    //places correct guess in game
					message = 'You got a letter right!';
          		}
        	})
    	} else {
		message = `Sorry, ${arg} is not in this word.`;
		guesses = [...guesses, arg];
  		}
win();  //checks for a win
}

//checks for win conditions and starts another game
const win = () => {
    if (compareArrays()) {
    chances = 0;
    message = wonGame;
	}
}
//converts arrays to string and returns boollean value
const compareArrays = () => {
let a = JSON.stringify(game);
let b = JSON.stringify(wordArr);
if (a === b){
    won = true;
    return won;
  }
}
start();  //kicks off the game and resets values

</script>

<main>
	<h1>Welcome to Hangman {name}</h1>
	<Inputs on:getlist={getList} />
    <h1>You have {chances} guesses.</h1>
	{#if chances}
	<span>Your guess:</span>
	<input bind:value={text} on:keydown={(event) => {if (event.key === 'Enter') play(text)} } >
	<button on:click={() => play(text)} >guess</button>
	{:else}
	<h2>{gameOver}</h2>
	{/if}
    <span>Your guesses: {guesses}</span>
    <p>Your last guess: {lastGuess}</p><p>Message: {message}</p>
	<div id="game">{game.join(' ')}</div>
	<button on:click={() => start()}>Reset</button>
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
		font-size: 2.5em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}

	#game {
        font-size: 100px;
      }
      span, p {
        font-size: 210%;
        padding-right: 10px;
      }
      input, button {
        font-size: 200%;
        width: 150px;
      }
</style>