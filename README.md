# PigDiceGame
Pig Dice JavaScript Game

## GAME RULES:

- The game has 2 players, playing in rounds
- In each turn, a player rolls a dice as many times as he whishes. Each result get added to his ROUND score
- BUT, if the player rolls a 1, all his ROUND score gets lost. After that, it's the next player's turn
- The player can choose to 'Hold', which means that his ROUND score gets added to his GLBAL score. After that, it's the next player's turn
- The first player to reach 100 points on GLOBAL score wins the game


### 3 MORE CHALLENGES
Change the game to follow these rules:

1. A player looses his ENTIRE score when he rolls two 6 in a row. After that, it's the next player's turn. (Hint: Always save the previous dice roll in a separate variable)
2. Add an input field to the HTML where players can set the winning score, so that they can change the predefined score of 100. (Hint: you can read that value with the .value property in JavaScript. This is a good oportunity to use google to figure this out :)
3. Add another dice to the game, so that there are two dices now. The player looses his current score when one of them is a 1. (Hint: you will need CSS to position the second dice, so take a look at the CSS code for the first one.)




#  [Udemy][Javascript]The Complete JavaScript Course 2018 by Jonas Schmedtmann

## JavaScript in the Browser: DOM Manipulation and Events

### The DOM and DOM Manipulation
* DOM: Document Object Model
* Structured representation of an HTML document;
* The DOM is used to connect webpages to script like JavaScript
* For each HTML box, there is an object that is the DOM that we can access and interaction with.
### First DOM Access and Manipulation
#### Set scores for each player 
```
var score1 = 0;
var score2 = 0;
```
Keep clean as one variable -> to use an array
One score at a time and one round score at a time
```
var scores = [0,0];
var roundScore = 0;
```
Declare all variables to keep more organized
```
var scores, roundScore;
scores = [0,0];
roundScore = 0;
```
Keep track who's currently playing
```
var scores, roundScore, activePlayer;
scores = [0,0];
roundScore = 0;
activePlayer = 0;
```
#### Calculate dice
Math.random() // => random number
Math.random() * 6  // => 0.XXXXXXXXXXXXXXX to 5.XXXXXXXXXXXXXXX
Math.floor() // => make decimals to integers
Math.floor(Math.random() * 6) + 1 // => from 1 to 6 integers
```
dice = Math.floor(Math.random() * 6) + 1;
```
#### DOM Manupulation -> Print current score
querySelector // => select class or id
textContent // => get the text content of an element
('#current-0') >>> ('#current-' + activePlayer ) // => dynamic printed in other layer 

```
document.querySelector('#current-' + activePlayer ).textContent = dice;
```
Another choice: innerHTML // => need to be a string
```
document.querySelector('#current-' + activePlayer ).innerHTML = '<em>' + dice + '</em>';
```
Setter vs. Getter
```
document.querySelector('#current-' + activePlayer ).textContent = dice; // => setter: Set a value

var x = document.querySelector('#score-0').textContent; // => getter: Get a value 
console.log(x);
```
querySelector of css property
```
document.querySelector('.dice').style.display = 'none';
```
### Events and Event Handling: Rolling the Dice
* Events: Notifications that are sent to that notify the code that something happened on the webpage;
* Examples: clicking a button, resizing a window, scrolling down or pressing a key;
* Event listener: A function that performs an action that based on a certain event. It waits for a specific events to happen.
![](https://i.imgur.com/xj1WuWy.png)
![](https://i.imgur.com/L3Jyapd.jpg)

#### anonymouse function: cannot use outside of this context
```
document.querySelector('.btn-roll').addEventListener('click', funciton(){ 
    // 1. randome number
    // 2. Display the result    
    // 3. Update the round score IF the rolled number was not a "1"
});
```
1. randome number
```
    var dice = Math.floor(Math.random() * 6) + 1;
```
2. Display the result   
```
    var diceDOM = document.querySelector('.dice');
    diceDOM.style.display = 'block';
    diceDOM.src = 'dice-' + dice + '.png';
```
#### getElementById could be a little bit faster than querySelector 
```
document.getElementById('score-0').textContent = '0';
document.getElementById('score-1').textContent = '0';
document.getElementById('current-0').textContent = '0';
document.getElementById('current-1').textContent = '0';
```

#### Total code
```
var scores, roundScore, activePlayer;
scores = [0,0];
roundScore = 0;
activePlayer = 0;

document.querySelector('.dice').style.display = 'none';
document.getElementById('score-0').textContent = '0';
document.getElementById('score-1').textContent = '0';
document.getElementById('current-0').textContent = '0';
document.getElementById('current-1').textContent = '0';

document.querySelector('.btn-roll').addEventListener('click', function(){
    // 1. randome number
    var dice = Math.floor(Math.random() * 6) + 1;
    // 2. Display the result
    var diceDOM = document.querySelector('.dice');
    diceDOM.style.display = 'block';
    diceDOM.src = 'dice-' + dice + '.png';
    // 3. Update the round score IF the rolled number was not a "1"

});
```

### Updating Scores and Changing the Active Player

#### != -> type coercion !== -> not trigger type coercion
```
    // 3. Update the round score IF the rolled number was not a "1"
    If (dice !== 1){
        //Add score
    }else{
        //Next Player
    }
```
#### roundScore = roundScore + dice; -> roundScore += dice;
#### display it in the user interface
```
 if (dice !== 1){
        //Add score
        roundScore += dice;
        document.querySelector('#current-' + activePlayer).textContent = roundScore;
    } else {
        //Next Player
    }
```
#### ternary operator
```
    if(activePlayer === 0){
        activePlayer = 1;
    } else {
        activePlayer = 0;
    } // => too much
```
```
    activePlayer === 0 ? activePlayer = 1 : activePlayer = 0; // => much cleaner
```
#### Clean the score and display it in the user interface
```
roundScore = 0;
        document.getElementById('current-0').textContent = 0;
        document.getElementById('current-1').textContent = 0;
```
#### Change active class(but cannot toggle)
```
    document.querySelector('.player-0-panel').classList.remove('active');
    document.querySelector('.player-1-panel').classList.add('active');
```
#### Toggle active class
```
    document.querySelector('.player-0-panel').classList.toggle('active');
    document.querySelector('.player-1-panel').classList.toggle('active');
```
#### when a player rolls a one, to hide the dice
```
document.querySelector('.dice').style.display = 'none';
```
### Implementing Our 'Hold' Function and the DRY Principle
* How to use functions to correctly apply the DRY(Don't repeat yourself) principle
* How to think about game logic like a programmer
```
document.querySelector('.btn-hold').addEventListener('click', function(){
    // Add CURRENT score to GLOBAL score
    // Update the UI
    // Check if player own the game
});
```
1. Add CURRENT score to GLOBAL score
```
scores[activePlayer] += roundScore; // => use the curely brackets to access the variables or to mutate it
```
2. Update the UI
```
document.querySelector('#score-' + activePlayer).textContent = scores[activePlayer];
```
4. Next Player
```
    activePlayer === 0 ? activePlayer = 1 : activePlayer = 0;
    roundScore = 0;
    document.getElementById('current-0').textContent = 0;
    document.getElementById('current-1').textContent = 0;
    document.querySelector('.player-0-panel').classList.toggle('active');
    document.querySelector('.player-1-panel').classList.toggle('active');
    document.querySelector('.dice').style.display = 'none';
```
#### But we don't want to repeat the code, we add a new function called nextPlayer, add ```nextPlayer();``` to instead
```
function nextPlayer(){
    activePlayer === 0 ? activePlayer = 1 : activePlayer = 0;
    roundScore = 0;
    document.getElementById('current-0').textContent = 0;
    document.getElementById('current-1').textContent = 0;
    document.querySelector('.player-0-panel').classList.toggle('active');
    document.querySelector('.player-1-panel').classList.toggle('active');
    document.querySelector('.dice').style.display = 'none';
}
```
```
document.querySelector('.btn-hold').addEventListener('click', function(){
    // Add CURRENT score to GLOBAL score
    scores[activePlayer] += roundScore; // => use the curely brackets to access the variables or to mutate it 
    // Update the UI
    document.querySelector('#score-' + activePlayer).textContent = scores[activePlayer];
    // Check if player own the game
    nextPlayer();
});
```
3. Check if player win the game
```
    if ( scores[activePlayer] >= 100 ){
        document.querySelector('#name-' + activePlayer).textContent = 'Winner!';
    } else {
        //Next Player
        nextPlayer();
    }
```
#### hide the dice when someone wins and add winner class
```
    document.querySelector('.dice').style.display = 'none';
    document.querySelector('.player-' + activePlayer + '-panel').classList.add('winner');
    document.querySelector('.player-' + activePlayer + '-panel').classList.remove('active');
```
#### Total Code
```
document.querySelector('.btn-hold').addEventListener('click', function(){
    // Add CURRENT score to GLOBAL score
    scores[activePlayer] += roundScore; // => use the curely brackets to access the variables or to mutate it 
    // Update the UI
    document.querySelector('#score-' + activePlayer).textContent = scores[activePlayer];
    // Check if player own the game
    if ( scores[activePlayer] >= 100 ){
        document.querySelector('#name-' + activePlayer).textContent = 'Winner!';
        document.querySelector('.dice').style.display = 'none';
        document.querySelector('.player-' + activePlayer + '-panel').classList.add('winner');
        document.querySelector('.player-' + activePlayer + '-panel').classList.remove('active');
    } else {
        //Next Player
        nextPlayer();
    }
});
function nextPlayer(){
    //Next Player
    activePlayer === 0 ? activePlayer = 1 : activePlayer = 0;
    roundScore = 0;
    document.getElementById('current-0').textContent = 0;
    document.getElementById('current-1').textContent = 0;

    document.querySelector('.player-0-panel').classList.toggle('active');
    document.querySelector('.player-1-panel').classList.toggle('active');
    document.querySelector('.dice').style.display = 'none';
}
```
### Creating a Game Initialization Function
```
document.querySelector('.btn-new').addEventListener('.click', function(){
    scores = [0,0];
    roundScore = 0;
    activePlayer = 1;
})
```
#### use a init function to follow DRY principle here
```
function init(){
    scores = [0,0];
    roundScore = 0;
    activePlayer = 1;

    document.querySelector('.dice').style.display = 'none';
    
    document.getElementById('score-0').textContent = '0';
    document.getElementById('score-1').textContent = '0';
    document.getElementById('current-0').textContent = '0';
    document.getElementById('current-1').textContent = '0';
}
```
#### And call init function when click new game button
```
document.querySelector('.btn-new').addEventListener('click', function(){
    init();
})
```
#### not call a anonymouse funciton, instead our init function. 
#### Don't need to use a call function here. We don't need to call the funciton immediately here, we just want to tell this event listener, when someone click the button, call the function for me.
```
document.querySelector('.btn-new').addEventListener('click', init);
```
#### Clean active and winner class, and set first player to be active player
```
    document.querySelector('.player-0-panel').classList.remove('winner');
    document.querySelector('.player-1-panel').classList.remove('winner');
    document.querySelector('.player-0-panel').classList.remove('active');
    document.querySelector('.player-1-panel').classList.remove('active');
    document.querySelector('.player-0-panel').classList.add('active');
```
### Finishing Touches: State Variables
#### State Variables tell us the condition of a system 
#### All the functions only access to the global scope or the scope of their parents. We defined the gamePlaying in the globale scope, then it can be used in other scope
```
var scores, roundScore, activePlayer, gamePlaying;
```
```
function init(){
    scores = [0,0];
    roundScore = 0;
    activePlayer = 1;
    gamePlaying = true;
    .
    .
    .
    .
    .
}
```
#### add an if statement of btn-hold and btn-roll
```
document.querySelector('.btn-roll').addEventListener('click', function(){
    if(gamePlaying){
        .
        .
        .
    }
```
```
document.querySelector('.btn-hold').addEventListener('click', function(){
    if(gamePlaying){
        .
        .
        .
    }
}
```
#### gamePlaying be false when someone wins
```
if ( scores[activePlayer] >= 100 ){
        document.querySelector('#name-' + activePlayer).textContent = 'Winner!';
        document.querySelector('.dice').style.display = 'none';
        document.querySelector('.player-' + activePlayer + '-panel').classList.add('winner');
        document.querySelector('.player-' + activePlayer + '-panel').classList.remove('active');
        gamePlaying = false;
    
        } else {
            nextPlayer();
        }
```
#### All Codes
```
var scores, roundScore, activePlayer, gamePlaying;
init();

document.querySelector('.btn-roll').addEventListener('click', function(){
    if(gamePlaying){
        // 1. randome number
        var dice = Math.floor(Math.random() * 6) + 1;
        // 2. Display the result
        var diceDOM = document.querySelector('.dice');
        diceDOM.style.display = 'block';
        diceDOM.src = 'dice-' + dice + '.png';
        // 3. Update the round score IF the rolled number was not a "1"
        if (dice !== 1){
            //Add score
            roundScore += dice;
            document.querySelector('#current-' + activePlayer).textContent = roundScore;
        } else {
            //Next Player
            nextPlayer();
        }
    }
});
document.querySelector('.btn-hold').addEventListener('click', function(){
    if(gamePlaying){
        // Add CURRENT score to GLOBAL score
        scores[activePlayer] += roundScore; // => use the curely brackets to access the variables or to mutate it 
        // Update the UI
        document.querySelector('#score-' + activePlayer).textContent = scores[activePlayer];
        // Check if player own the game
        if ( scores[activePlayer] >= 20 ){
            document.querySelector('#name-' + activePlayer).textContent = 'Winner!';
            document.querySelector('.dice').style.display = 'none';
            document.querySelector('.player-' + activePlayer + '-panel').classList.add('winner');
            document.querySelector('.player-' + activePlayer + '-panel').classList.remove('active');
            gamePlaying = false;
    
        } else {
            //Next Player
            nextPlayer();
        }
    }
});
function nextPlayer(){
    //Next Player
    activePlayer === 0 ? activePlayer = 1 : activePlayer = 0;
    roundScore = 0;
    document.getElementById('current-0').textContent = 0;
    document.getElementById('current-1').textContent = 0;

    document.querySelector('.player-0-panel').classList.toggle('active');
    document.querySelector('.player-1-panel').classList.toggle('active');
    document.querySelector('.dice').style.display = 'none';
}
document.querySelector('.btn-new').addEventListener('click', init);

function init(){
    scores = [0,0];
    roundScore = 0;
    activePlayer = 1;
    gamePlaying = true;

    document.querySelector('.dice').style.display = 'none';

    document.getElementById('score-0').textContent = '0';
    document.getElementById('score-1').textContent = '0';
    document.getElementById('current-0').textContent = '0';
    document.getElementById('current-1').textContent = '0';

    document.getElementById('name-0').textContent = 'Player 1';
    document.getElementById('name-1').textContent = 'Player 2';

    document.querySelector('.player-0-panel').classList.remove('winner');
    document.querySelector('.player-1-panel').classList.remove('winner');
    document.querySelector('.player-0-panel').classList.remove('active');
    document.querySelector('.player-1-panel').classList.remove('active');
    document.querySelector('.player-0-panel').classList.add('active');
}
```
### Coding Challenge 3
#### Topic

YOUR 3 CHALLENGES
Change the game to follow these rules:

1. A player looses his ENTIRE score when he rolls two 6 in a row. After that, it's the next player's turn. (Hint: Always save the previous dice roll in a separate variable)
2. Add an input field to the HTML where players can set the winning score, so that they can change the predefined score of 100. (Hint: you can read that value with the .value property in JavaScript. This is a good oportunity to use google to figure this out :)
3. Add another dice to the game, so that there are two dices now. The player looses his current score when one of them is a 1. (Hint: you will need CSS to position the second dice, so take a look at the CSS code for the first one.)

### Coding Challenge 3: Solution, Part 1
```
var lastDice;

document.querySelector('.btn-roll').addEventListener('click', function(){
    if (gamePlaying){
        .
        .
        .
        .
        if (dice === 6 && lastDice === 6 ){
            scores[activePlayer] = 0;
            document.querySelector('#score-' + activePlayer).textContent = '0';
            nextPlayer();
        } else if ( dice !== 1){
            roundScore += dice;
            document.querySelector('#current-' + activePlayer).textContent = roundScore;
        } else {
            // next player
            nextPlayer();
        }
        lastDice = dice;
    }
});
```

### Coding Challenge 3: Solution, Part 2

```
document.querySelector('.btn-hold').addEventListener('click', function(){
    if (gamePlaying){
        .
        .
        .
        var input = document.querySelector('.final-score').value;
        console.log(input);
        var winningScore;
        // undefined, 0, null or "" are COERCED to flase
        // Anything else is COERCED to true => 
        if (input){
            var winningScore = input;
        } else {
            winningScore = 100;
        }
        // Check if player own the game
        if ( scores[activePlayer] >= winningScore){
            document.querySelector('#name-' + activePlayer).textContent = 'Winner!';
            document.querySelector('.player-' + activePlayer + '-panel').classList.add('winner');
            gamePlaying = false;
        } else {
            nextPlayer();
        }
    }
});
```
### Coding Challenge 3: Solution, Part 3

```
var scores, roundScore, activePlayer, gamePlaying;
init();

document.querySelector('.btn-roll').addEventListener('click', function(){
    if (gamePlaying){
        // 1. randome number
        var dice1 = Math.floor(Math.random() * 6) + 1;
        var dice2 = Math.floor(Math.random() * 6) + 1;
        // 2. Display the result    
        document.getElementById('dice-1').style.display = 'block';
        document.getElementById('dice-2').style.display = 'block';
        document.getElementById('dice-1').src = 'dice-' + dice1 + '.png';
        document.getElementById('dice-2').src = 'dice-' + dice2 + '.png';
        // 3. Update the round score IF the rolled number was not a "1"

        if (dice1 === 6 && dice2 === 6 ){
            scores[activePlayer] = 0;
            document.querySelector('#score-' + activePlayer).textContent = '0';
            nextPlayer();
        } else if ( dice1 !== 1 && dice2 !== 1 ){
            roundScore += dice1 + dice2;
            document.querySelector('#current-' + activePlayer).textContent = roundScore;
        } else {
            // next player
            nextPlayer();
        }
        dice2 = dice1;
    }
});
document.querySelector('.btn-hold').addEventListener('click', function(){
    if (gamePlaying){
        // Add CURRENT score to GLOBAL score
        scores[activePlayer] += roundScore;
        // Update the UI
        document.querySelector('#score-' + activePlayer).textContent = scores[activePlayer];
        
        var input = document.querySelector('.final-score').value;
        console.log(input);
        var winningScore;
        // undefined, 0, null or "" are COERCED to flase
        // Anything else is COERCED to true => 
        if (input){
            var winningScore = input;
        } else {
            winningScore = 100;
        }
        // Check if player own the game
        if ( scores[activePlayer] >= winningScore){
            document.querySelector('#name-' + activePlayer).textContent = 'Winner!';
            document.querySelector('.player-' + activePlayer + '-panel').classList.add('winner');
            gamePlaying = false;
        } else {
            nextPlayer();
        }
    }
});
document.querySelector('.btn-new').addEventListener('click', init);

function nextPlayer(){
    activePlayer === 0? activePlayer = 1 : activePlayer = 0;
    roundScore = 0;
    document.getElementById('current-0').textContent = '0';
    document.getElementById('current-1').textContent = '0';
    document.querySelector('.player-0-panel').classList.toggle('active');
    document.querySelector('.player-1-panel').classList.toggle('active');
    // document.getElementById('dice-1').style.display = 'none';
    // document.getElementById('dice-2').style.display = 'none';
}
function init(){
    scores = [0, 0];
    roundScore = 0;
    activePlayer = 0;
    gamePlaying = true;
    document.getElementById('score-0').textContent = '0';
    document.getElementById('score-1').textContent = '0';
    document.getElementById('current-0').textContent = '0';
    document.getElementById('current-1').textContent = '0';
    
    document.getElementById('name-0').textContent = 'Player 1';
    document.getElementById('name-1').textContent = 'Player 2';

    document.querySelector('.player-0-panel').classList.remove('winner');
    document.querySelector('.player-1-panel').classList.remove('winner');
    document.querySelector('.player-0-panel').classList.remove('active');
    document.querySelector('.player-1-panel').classList.remove('active');
    document.querySelector('.player-0-panel').classList.add('active');
    // document.getElementById('dice-1').style.display = 'none';
    // document.getElementById('dice-2').style.display = 'none';

};
```
