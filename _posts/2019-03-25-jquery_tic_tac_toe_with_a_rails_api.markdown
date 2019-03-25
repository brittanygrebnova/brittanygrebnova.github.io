---
layout: post
title:      "jQuery Tic Tac Toe with a Rails API"
date:       2019-03-25 16:20:05 +0000
permalink:  jquery_tic_tac_toe_with_a_rails_api
---


This was, without a doubt, the most difficult lab to date. I asked for help quite a few times, but each time I had a better understanding of what my code could do and what was still missing. Here, I'll touch on a few concepts that I encountered during this assignment that at first baffled me, but then became more familiar by the end when I finally submitted the lab.

**CHECKING THE EQUALITY OF ELEMENTS WITHIN AN ARRAY**

We needed to iterate through a nested array of win combinations in order to see if either player X or player O had 3 in a row. Seems easy, but it held me up for some time and it wasn't until I tried many unsuccessful blocks of code that I was finally able to piece this checkWinner function together:

```
function checkWinner() {
  const winCombinations = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [6, 4, 2],
  ]

  var winner = false

  const tds = $("td")

  winCombinations.forEach(function(combo) {
    if (tds[combo[0]].innerHTML !== "" && tds[combo[0]].innerHTML === tds[combo[1]].innerHTML && tds[combo[1]].innerHTML === tds[combo[2]].innerHTML) {
      setMessage(`Player ${ tds[combo[0]].innerHTML } Won!`)
      winner = true
      saveGame()
    }
  });
```

The winCombinations nested array contains individual arrays that represent 3 consecutive squares on the board. For instance, [0, 1, 2] would be the top row and [0, 3, 6] would be the left column. We define the variable tds to equal the jQuery result of all the td elements (squares on the board). We iterate over each element in tds and first check that the first position's innerHTML is not blank. Then we compare the first, second, and third positions' innerHTML and, if they're all equal, we set a message equal to the innerHTML of the matching elements (either X or O) to the player that won. This function was supposed to return either true or false so, by default, it returns false unless a winner is found. At the very end, we call saveGame() which is a function that also took quite some time to work out...

**USING AJAX TO SEND A PATCH REQUEST**

I didn't find a ton of resources on the topic but the method for executing this request is not very difficult. Here's my saveGame function:

```
function saveGame() {
  if (!!currentGame) {
    $.ajax({
      type: 'PATCH',
      url: `/games/${currentGame}`,
      data: {state:board}
    })
  } else {
    $.post('/games', {state:board}, function(data) {
      currentGame = data.data.id
    })
  }
}
```

The currentGame global variable is set equal to 0 by default. When the user clicks the save button for the first time and the code is run to check for currentGame, it returns falsey and sends a post request to '/games'. The state of the board (the innerHTML of each square) is sent with the request and the data variable returns the id of the newly saved game. Then currentGame is then set to the id and is no longer 0. If the user clicks save game again on a game that was already saved, the code runs to send a patch request to '/games/' with the game id appended to the end of the url with the state of the board sent as data to update the game in the database. Cool stuff!

**ARRAY FROM NODE LIST**

One more concept that went from blurry to moderately understandable for me in this lab was the return values of DOM queries. In order to grab the state of the board, I defined a board variable and performed querySelectorAll on it. But the return value is a series of node objects, not an array. And the game model expects state to be defined as an array, so I was gifted the knowledge of the Array.from method. Here is how I grabbed the state of the board and turned it into a pretty array:

```
let board = Array.from(document.querySelectorAll("td")).map(td => td.innerHTML)
```

That line of code returns something like this, depending on which squares are filled with either X or O:

```
["X", "", "", "O", "", "", "", "", ""]
```

and the state of the board can be saved!


It was fun to build a Rails/JS version of the tic tac toe lab that we previously built with plain Ruby. Now I move on to adding JS to my Rails app for this section's final project!

The process of learning to code is absolutely mind-blowing; the longer you continue to learn, the more you find you don't know. It's a deep, wide rabbit hole with millions of pieces of information just waiting to be discovered.

Thank you for reading!
