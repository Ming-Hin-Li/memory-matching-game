<!DOCTYPE html>
<html>

<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8"> 
	
    <title>Memory Matching Game</title> 

    <style>
        * {
            font-family: Arial; 
        }

        header, footer {
            background-color: #666666; 
            color: #FFFFFF; 
            text-align: center; 
            padding: 10px; 
        }

        main {
            padding: 10px 0; 
        }

        .grid-container {
            display: grid; 
            grid-template-columns: repeat(3, 1fr);
            grid-gap: 10px; 
            width: 240px;
            margin: auto; 
        }
        
        .grid-item {
            display: grid; 
            width: 80px; 
            height: 80px; 
            justify-content: center; 
            align-items: center; 
            background-color: #F0F0F0; 
            cursor: pointer; 
            font-size:60px;
            text-indent: 1000px;
            overflow: hidden;
        }
      

        .yellow {
            text-indent: 0px;
        }

        .green {
            text-indent: 0px; 
        }
    </style>
</head>

<body>

<header>
	<h1>Memory Matching Game</h1> 
</header>

<main>
    <div class="grid-container" id="grid-container">
    </div>
</main>

<footer>
    <b>單元五練習</b> 
</footer>

<script>
    const emoji = ['😭','😁','😉','☺️','😂','😩','😔','😳'];
      const cards = [...emoji, ...emoji];
      var clickedCard = null;
      var matchedCards = 0;
      var firstClick = true;
      var gameStarted = false;
      function toggleGridSelection(event) {
        var grid = event.target; 
        if (!gameStarted) {
          shuffle(cards);
          gameStarted = true;
        }
        if (grid.classList.contains('yellow') || grid.classList.contains('green')) {
          return;
        }
        grid.classList.add('yellow');
        if (clickedCard === null) {
          clickedCard = grid;
          return;
        }
        if (clickedCard.textContent == grid.textContent) {
          clickedCard.classList.remove('yellow');
          grid.classList.remove('yellow');
          clickedCard.classList.add('green');
          grid.classList.add('green');
          clickedCard = null;
          matchedCards += 2;
          if (matchedCards === cards.length) {
            setTimeout(() => {
              alert('You win!');
              resetGame();
            }, 500);
          }
        } else {
          setTimeout(() => {
            clickedCard.classList.remove('yellow');
            grid.classList.remove('yellow');
            clickedCard = null;
          }, 1000);
        }
      }
      function shuffle(array) {
        for (let i = array.length - 1; i > 0; i--) {
          const j = Math.floor(Math.random() * (i + 1));
          [array[i], array[j]] = [array[j], array[i]];
        }
      }
      function resetGame() {
        clickedCard = null;
        matchedCards = 0;
        firstClick = true;
        gameStarted = true;
        document.querySelectorAll('.grid-item').forEach(function (grid) {
          grid.classList.remove('yellow');
          grid.classList.remove('green');
          grid.textContent = '';
        });
        for (let i = 1; i <= gridSize * gridSize; i++) {
          const gridItem = document.querySelectorAll('.grid-item')[i - 1];
          gridItem.textContent = cards[i - 1];
        }
      }
      const gridSize = 4; 
      const gridContainer = document.getElementById('grid-container');
      gridContainer.style.gridTemplateColumns = `repeat(${gridSize}, 1fr)`;
      gridContainer.style.width = `${gridSize * 80 + (gridSize - 1) * 10}px`;
      for (let i = 1; i <= gridSize * gridSize; i++) {
        const gridItem = document.createElement('div');
        gridItem.className = 'grid-item';
        gridItem.textContent = cards[i - 1];
        gridContainer.appendChild(gridItem);
      }
      document.querySelectorAll('.grid-item').forEach(function (grid) {
        grid.addEventListener('click', toggleGridSelection);
      });
</script>

</body>
</html>
