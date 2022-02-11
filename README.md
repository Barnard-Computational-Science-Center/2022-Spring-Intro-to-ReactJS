## Introduction to ReactJS - Spring 2022

##### What is React?
- A JavaScript library for building user interfaces
- Framework for developing UI components

##### Why use React?
- Useful for developing complex, single-page-applications (SPAs).
- Moving away from directly manipulating HTML/DOM nodes with event listeners, etc.
- Component and data driven development.

##### Prerequisites

- [Install NodeJS](https://nodejs.org/en/)
- Text editor with support for JSX (VS Code, Sublime with Babel plugin)

##### Goals

- Learn to handle and manipulate data, functions/hooks, basic state management, and working with 3rd party libraries.

##### Process
- Simplify your idea.
- Prototype, fail quickly, iterate.
- Don't get stuck in a local minima.


---

### Create React App

After installing NodeJS, open a terminal (`Powershell` on PC), `cd` into your working directory, and run the following commands (each individually):

	npx create-react-app react-workshop-app
	cd react-workshop-app
	npm start


Open the project in your editor (I'm using Visual Studio Code):

	Visual Studio Code 2 > File > Open Folder > ...react-workshop-app > Open


#### App.js 
~~~~javascript
import React from 'react';

function App() {
  return (
    <div>
      <h1>
         Hello!
      </h1>
    </div>
  );
}

export default App;
~~~~

### Install Tailwind CSS 
[Official documentation.](https://tailwindcss.com/docs/guides/create-react-app)

1. First, stop the app by navigating to your terminal and pressing `control + c`

2. In your terminal (making sure you're in the project folder `react-workshop-app`), install Tailwind CSS:

    ``` 
    npm install -D tailwindcss postcss autoprefixer 
    ```

3. Create the Tailwind CSS config file by executing the following command (note the `npx` not `npm`):

    ```
    npx tailwindcss init
    ```
4. Update the newly created `tailwind.config.js` file with the following and save:

    ```javascript
    module.exports = {
      content: [
        "./src/**/*.{js,jsx,ts,tsx}",
      ],
      theme: {
        extend: {},
      },
      plugins: [],
    }
    ```
6. Open `/src/index.css` and add the Tailwind directives (you can remove the original styles):

    ```css
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
    ```

7.  In your terminal, run the build scripts: 
    ```
    npm run start
    ```

---
### 1. Back to the app...

- You may have noticed that the `h1` tag inside `App.js` has lost its styles. 
- Tailwind ships with an opinionated (but configurable) set of styles that normalize some tags, like headers. [You can read more about it here](https://tailwindcss.com/docs/preflight).


#### 1.1 App.js

- Using Tailwind Utility classes to style the `<h1>` tag

```javascript
function App() {
  return (
    <div>
      <h1 className="text-3xl text-blue-700 font-bold">
        Hello! World..
      </h1>
    </div>
  );
}

export default App;
```


#### 1.2 Application state
Application state
- In React, we use "state" variables to control data flow. React components "rerender" on every change in our "state" variables.
- State is a piece of data stored in our component. For example, the "state" of a "door" can either be "open" or "closed". We could represent that in React: `isDoorOpen=true`

#### 1.3 App.js
- Import `React` & `useState`
- Declare a state variable
- Create a `<button>`
- Attach a handler
- Render click count
- `console.log` count value

```javascript
import React, { useState } from "react";

function App() {
  const [count, setCount] = useState(0);

  function handleClick() {
    console.log(count);
  }

  return (
    <div>
      <h1 className="text-3xl font-bold text-blue-700">
        Hello! World..
      </h1>
      <p>Clicks: {count}</p>
      <button onClick={handleClick}>Click</button>
    </div>
  );
}

export default App;
```




#### 1.4 App.js
- Increment `count` on click.
- Style the `<button>`, `<p>`.

```javascript
import React, { useState } from "react";

function App() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1 className="text-3xl font-bold text-blue-700">Hello! World..</h1>
      <p className="my-10">Clicks: {count}</p>
      <button
        className="bg-green-800 px-2 py-2 text-white"
        onClick={handleClick}
      >
        Click
      </button>
    </div>
  );
}

export default App;
```



#### 1.5 Why React?
- What are the benefits of the "React" approach? What even is the "React" way of doing things?
    - Composability - We can take smaller pieces of UI and "compose" larger structures while minimizing complexity.
    - By focusing on state and the underlying data model, we don't worry about manipulating the DOM (Document Object Model) / HTML directly. The rendered HTML will always be a function of our application state.
    - While opinions differ, the "declarative" coding style, along with the influence of "functional" programming make it a highly productive environment. UI components are reduced to simple JavaScript functions, which can be easier to reason about.


---

### 2. Composition

- Make `<button>` its own component called `CounterButton`.
- React components must start with a capital letter.
- We can pass object (values, functions, etc.) as `props` to `CounterButton`
- Data flows one-way in React, from parent to child components.


#### 2.1 App.js
```javascript
import React, { useState } from "react";

function CounterButton(props) {
  const { text, step, count, setCount, styles } = props;

  function handleClick() {
    setCount(count + step);
  }

  return (
    <button className={styles} onClick={handleClick}>
      {text}
    </button>
  );
}

function App() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1 className="text-3xl font-bold text-blue-700">Hello! World..</h1>
      <p className="my-10">Clicks: {count}</p>

      <CounterButton
        text={"Decrement"}
        step={-1}
        count={count}
        setCount={setCount}
        styles={"bg-red-800 px-2 py-2 text-white"}
      />

      <CounterButton
        text={"Increment"}
        step={1}
        count={count}
        setCount={setCount}
        styles={"bg-green-800 px-2 py-2 text-white"}
      />
    </div>
  );
}

export default App;

```


#### 2.2 CounterButton.js

- Put `CounterButton.js` in its own file and `export default`

```javascript
export default function CounterButton(props) {
  const { text, step, count, setCount, styles } = props;

  function handleClick() {
    setCount(count + step);
  }

  return (
    <button className={styles} onClick={handleClick}>
      {text}
    </button>
  );
}
```

#### 2.3 App.js

- Import `CounterButton` component.

```javascript
import React, { useState } from "react";
import CounterButton from "./CounterButton";

function App() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1 className="text-3xl font-bold text-blue-700">
        Hello! World..
      </h1>
      <p className="my-10">Clicks: {count}</p>

      <CounterButton
        text={"Decrement"}
        step={-1}
        count={count}
        setCount={setCount}
        styles={"bg-red-800 px-2 py-2 text-white"}
      />

      <CounterButton
        text={"Increment"}
        step={1}
        count={count}
        setCount={setCount}
        styles={"bg-green-800 px-2 py-2 text-white"}
      />
    </div>
  );
}

export default App;
```




### 3. Working with data, declarative programming

- [Water Consumption in the City of New York](https://data.cityofnewyork.us/Environment/Water-Consumption-in-the-City-of-New-York/ia2d-e54m)
- Go to API > Copy JSON endpoint. 
- Paste endpoint URL in browser, copy the data (`command + a`).
- Create a new file inside `/src` called `data.json`
- Paste the data to this file and save.
- Variables are:
    - new_york_city_population
    - nyc_consumption_million_gallons_per_day
    - per_capita_gallons_per_person_per_day
    - year


#### 3.1 App.js
- Remove old code
- import data from `data.json`
- `map` through the data array and return a `<li>` for each year

```javascript
import React, { useState } from "react";
import data from "./data.json";

function App() {
  return (
    <div>
      <ul>
        {data.map((obj) => (
          <li>{obj.year}</li>
        ))}
      </ul>
    </div>
  );
}

export default App;

```

#### 3.2 App.js

- Not great, but better?

```javascript
import React, { useState } from "react";
import data from "./data.json";

function App() {
  return (
    <div className="">
      <h3 className="mx-auto max-w-sm border border-2 border-gray-800 bg-lime-300 px-4 py-2 text-lg font-medium text-gray-900">
        Water Consumption in New York City
      </h3>

      <ul className="px-10 grid grid-cols-12 gap-4 ">
        {data.map((obj) => (
          <li className="my-2 border-2 border-gray-800">
            <div> {obj.year}</div>
            <div> {obj.new_york_city_population}</div>
            <div> {obj.nyc_consumption_million_gallons_per_day}</div>
            <div> {obj.per_capita_gallons_per_person_per_day}</div>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default App;

```

#### 3.3 App.js

- More styles...
- Can you make it responsive (consider how it looks on different device sizes).
- Test devices on Chrome: `Right click` > `Inspect` > `Device Icon (Second button on top left)`


```javascript
import React, { useState } from "react";
import data from "./data.json";

function App() {
  return (
    <div className="bg-red-50">
      <h3 className="mx-auto max-w-sm border border-2 border-gray-800 bg-lime-300 px-4 py-2 text-center text-lg font-medium text-gray-900">
        Water Consumption in New York City
      </h3>

      <ul className="grid grid-cols-8 gap-4 px-10 ">
        {data.map((obj) => (
          <li className="col-span-8 my-2 border-2 border-gray-800 md:col-span-2 ">
            <h3 className="border-b-2 border-gray-800 bg-cyan-800 p-2 text-lg font-semibold text-white">
              {obj.year}
            </h3>

            <div className="bg-red-200">
              <div className="flex items-center justify-between p-4 hover:bg-red-300">
                <p className="truncate font-medium text-gray-600">Population</p>
                <div className="font-medium text-gray-800 underline">
                  {obj.new_york_city_population}
                </div>
              </div>

              <div className="flex items-center justify-between p-4 hover:bg-red-300">
                <p className="truncate font-medium text-gray-600">
                  Consumption
                </p>
                <div className="font-medium text-gray-800 underline">
                  {obj.nyc_consumption_million_gallons_per_day}
                </div>
              </div>

              <div className="flex items-center justify-between p-4 hover:bg-red-300">
                <p className="truncate font-medium text-gray-600">
                  Per capita, per person
                </p>
                <div className="font-medium text-gray-800 underline hover:bg-red-300">
                  {obj.per_capita_gallons_per_person_per_day}
                </div>
              </div>
            </div>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default App;

```



#### 3.4 App.js
- Split out a `Card` component
- Add a `Remove` button
- Attach a `onClick` handler (`handleClick`)


```javascript

import React, { useState } from "react";
import data from "./data.json";

function App() {
  const [cards, setCards] = useState(data);

  function handleClick(year) {
    const filteredCards = cards.filter((card) => card.year !== year);
    setCards(filteredCards);
  }

  return (
    <div className="bg-red-50">
      <h3 className="mx-auto max-w-sm border border-2 border-gray-800 bg-lime-300 px-4 py-2 text-center text-lg font-medium text-gray-900">
        Water Consumption in New York City
      </h3>

      <ul className="grid grid-cols-8 gap-4 px-10 ">
        {cards.map((obj) => (
          <Card handleClick={handleClick} obj={obj} />
        ))}
      </ul>
    </div>
  );
}

export default App;

function Card({ obj, handleClick }) {
  return (
    <li className="col-span-8 my-2 border-2 border-gray-800 md:col-span-2 ">
      <div className="flex items-center justify-between bg-cyan-800">
        <h3 className="border-b-2 border-gray-800  p-2 text-lg font-semibold text-white">
          {obj.year}
        </h3>

        <button
          onClick={() => handleClick(obj.year)}
          className="h-10 rounded-full border-4 border-double bg-teal-700 px-2 text-white"
        >
          Remove
        </button>
      </div>

      <div className="bg-red-200">
        <div className="flex items-center justify-between p-4 hover:bg-red-300">
          <p className="truncate font-medium text-gray-600">Population</p>
          <div className="font-medium text-gray-800 underline">
            {obj.new_york_city_population}
          </div>
        </div>

        <div className="flex items-center justify-between p-4 hover:bg-red-300">
          <p className="truncate font-medium text-gray-600">Consumption</p>
          <div className="font-medium text-gray-800 underline">
            {obj.nyc_consumption_million_gallons_per_day}
          </div>
        </div>

        <div className="flex items-center justify-between p-4 hover:bg-red-300">
          <p className="truncate font-medium text-gray-600">
            Per capita, per person
          </p>
          <div className="font-medium text-gray-800 underline hover:bg-red-300">
            {obj.per_capita_gallons_per_person_per_day}
          </div>
        </div>
      </div>
    </li>
  );
}

```

#### 3.5 Exercise 1

- Can you render the removed years somewhere? 
- You may need to use another state variable.

#### 3.6 Exercise 2
- Can you you add a button to "restore" the removed year?
