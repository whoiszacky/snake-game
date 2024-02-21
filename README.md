### NB:  
 
```js
/*
This was the less optimal approach I took
before following the blog tutorial.
*/
import React, { useState, useEffect } from 'react';

const numRows = 20;
const numCols = 20;

const App = () => {
  const [snake, setSnake] = useState([{ row: 10, col: 10 }]);
  const [food, setFood] = useState({ row: Math.floor(Math.random() * numRows), col: Math.floor(Math.random() * numCols) });
  const [direction, setDirection] = useState('RIGHT');
  const [gameOver, setGameOver] = useState(false);

  const handleKeyPress = (e) => {
    switch (e.key) {
      case 'ArrowUp':
        setDirection('UP');
        break;
      case 'ArrowDown':
        setDirection('DOWN');
        break;
      case 'ArrowLeft':
        setDirection('LEFT');
        break;
      case 'ArrowRight':
        setDirection('RIGHT');
        break;
      default:
        break;
    }
  };

  const moveSnake = () => {
    const head = snake[0];
    let newHead;

    switch (direction) {
      case 'UP':
        newHead = { row: head.row - 1, col: head.col };
        break;
      case 'DOWN':
        newHead = { row: head.row + 1, col: head.col };
        break;
      case 'LEFT':
        newHead = { row: head.row, col: head.col - 1 };
        break;
      case 'RIGHT':
        newHead = { row: head.row, col: head.col + 1 };
        break;
      default:
        break;
    }

    if (newHead.row < 0 || newHead.row >= numRows || newHead.col < 0 || newHead.col >= numCols || snake.some(segment => segment.row === newHead.row && segment.col === newHead.col)) {
      setGameOver(true);
      return;
    }

    const newSnake = [newHead, ...snake];
    if (newHead.row === food.row && newHead.col === food.col) {
      setFood({ row: Math.floor(Math.random() * numRows), col: Math.floor(Math.random() * numCols) });
    } else {
      newSnake.pop();
    }

    setSnake(newSnake);
  };

  const restartGame = () => {
    setSnake([{ row: 10, col: 10 }]);
    setFood({ row: Math.floor(Math.random() * numRows), col: Math.floor(Math.random() * numCols) });
    setDirection('RIGHT');
    setGameOver(false);
  };

  useEffect(() => {
    const intervalId = setInterval(() => {
      moveSnake();
    }, 100);

    document.addEventListener('keydown', handleKeyPress);

    return () => {
      clearInterval(intervalId);
      document.removeEventListener('keydown', handleKeyPress);
    };
  }, [snake, direction, food, gameOver]);

  if (gameOver) {
    return (
      <div>
        Game Over!
        <button onClick={restartGame}>Restart</button>
      </div>
    );
  }

  return (
    <div>
      {
        Array.from({ length: numRows }, (_, rowIndex) => (
          <div key={rowIndex} style={{ display: 'flex' }}>
            {
              Array.from({ length: numCols }, (_, colIndex) => (
                <div key={colIndex} style={{ width: '20px', height: '20px', backgroundColor: snake.some(segment => segment.row === rowIndex && segment.col === colIndex) ? 'black' : (food.row === rowIndex && food.col === colIndex ? 'red' : 'white') }} />
              ))
            }
          </div>
        ))
      }
    </div>
  );
};

export default App;
```
# Getting Started with Create React App

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in your browser.

The page will reload when you make changes.\
You may also see any lint errors in the console.

### `npm test`

Launches the test runner in the interactive watch mode.\
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `npm run build`

Builds the app for production to the `build` folder.\
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.\
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `npm run eject`

**Note: this is a one-way operation. Once you `eject`, you can't go back!**

If you aren't satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you're on your own.

You don't have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn't feel obligated to use this feature. However we understand that this tool wouldn't be useful if you couldn't customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

### Code Splitting

This section has moved here: [https://facebook.github.io/create-react-app/docs/code-splitting](https://facebook.github.io/create-react-app/docs/code-splitting)

### Analyzing the Bundle Size

This section has moved here: [https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size](https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size)

### Making a Progressive Web App

This section has moved here: [https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app](https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app)

### Advanced Configuration

This section has moved here: [https://facebook.github.io/create-react-app/docs/advanced-configuration](https://facebook.github.io/create-react-app/docs/advanced-configuration)

### Deployment

This section has moved here: [https://facebook.github.io/create-react-app/docs/deployment](https://facebook.github.io/create-react-app/docs/deployment)

### `npm run build` fails to minify

This section has moved here: [https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify](https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify)
