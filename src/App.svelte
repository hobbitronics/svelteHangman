<script>

let name = 'dude';
let chances;
let game = [];
let word;
let wordArr = [];
let another;
let won;
let text;
let lastGuess;
let guesses = [];
let message = '';
 
const wordList = [
    'apple', 'answer', 'anchor', 'bananna', 'berry', 'boat', 'boot', 'cape', 'cap', 'case', 'chip', 'cherry', 'cone', 'dark', 'deep', 'donkey', 'eight', 'ever', 'elephant', 'fire', 'food', 'good', 'great', 'hour', 'ink', 'jar', 'jump', 'juggle', 'kick', 'kangaroo', 'leopard', 'moon', 'night', 'opera', 'people', 'quiet', 'right', 'scissor', 'temple', 'unicorn', 'victorious', 'wink', 'xylophone', 'yelp', 'zebra'  //list game chooses from
];

//chooses a random word from the list as a string
const random = () => {
return wordList[Math.floor(Math.random()*wordList.length)];
}
//resets changes and gets a random word from the list
const start = () => {
	chances = 6;
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
	if (chances > 0) {
	lastGuess = arg.toLowerCase();
	guesses = [...guesses, arg];
	if (wordArr.includes(lastGuess)) {
		wordArr.forEach((el, index) => {
			if (el === lastGuess){
			game[index] = lastGuess;    //places correct guess in game
			message = 'You got a letter right!';
          			}
        		}
      		)
    	} else {
    	chances -=1;
    	message = `Sorry, ${arg} is not in this word.`;
  		}
	}
win();  //checks for a win
     if (!won && chances < 1) {
		message = `${wordArr.join('')} was the word. Game Over, Click Restart to play again.`
		  }
}

//checks for win conditions and starts another game
const win = () => {
    if (compareArrays()) {
    chances = 0;
    message = 'You won! play again? Click reset to play';
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
    <h1>You have {chances} guesses.</h1>
	<span>Your guess:</span>
	<input bind:value={text}>
    <button on:click={() => play(text)} >guess</button>
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