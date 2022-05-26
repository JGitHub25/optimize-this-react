# Advanced React: Optimization project Optimize This!

# Introduction

In this off-platform project, you will identify various performance bottlenecks using the React profiler and the Network tab in your browser’s developer tools. This project is a fully functioning Todo list application, however, it is plagued by expensive functions, large modules, and React code that causes needless re-renders. It’s your job to utilize your knowledge of React performance techniques to make this app fast and responsive.

## Task group 1: Project setup

To get started, download the project from above. Once downloaded, `cd` into the project directory and then into the `/starter` folder. Then install its dependencies with `npm install`.

This project was created with create-react-app.

In the command line, run `npm start` to start the app, then navigate to http://localhost:3000.

Once there, click around the app and see if you can identify any interactions that might be improved with a performance optimization.

## Task group 2: Memoize a value

In the app, click on the button in the top right corner that says "Anonymous Squirrel". Then, type in a new username and click on the question mark button to the right of "Username".

These interactions should feel sluggish. To track down which component is the culprit, open your browser's developer tools, then navigate to the React profiler. Once there, start recording a session. Type in the input field and click on the question mark button a few times.

In the resulting flame graph, identify the component that takes the longest time to render.

In the `src/components/Profile/Profile.js` component, there is a performance bottleneck with the username validation function. While the app needs to validate any new username, the app should not run the username validation function when clicking the question mark button.

Use a React hook to memoize the value of the `checkUsernameValidity()` function.

Now it's time to record another session with the React profiler to see how applying the performance optimization affected the app.

Open the React profiler and record another session while clicking the question mark button.

How does this session compare to the session before applying `useMemo()`?

## Task group 3: Memoize a component

Let's see how React is re-rendering components when we interact with the app.

Open the React profiler. In its settings, make sure that "Highlight updates when components render" is checked.

Start typing a new todo in the todo input and identify which components are highlighted when typing letters into the input field. Are there components re-rendering that do not need to re-render?

In the repository, navigate to `src/components/Todos/TodoItem.js` file. Since none of this component's props change when its parent changes, use a performance technique provided by React to memoize the `<TodoItem>` component.

Back in your browser, use the React profiler to highlight the components that are re-rendering.

Notice that when you type in todo input field, all the `<TodoItem>` components are still getting re-rendered by React on every keypress.

Why are the `<TodoItem>` components still re-rendering, even when they're memoized with `React.memo()`?

In the `<Todos>` component, identify which props it is passing to `<TodoItem>`. For each prop passed to `<TodoItem>`, identify the type of the prop (string, number, boolean, function, object, array, etc) it's being passed.

Which one of these props might be the one causing re-renders?

## Task group 4: Memoize a function

As it turns out, the `<TodoItem>` components are still re-rendering because they have the `formatTodoText()` function passed as a prop. In the `<Todos>` component, this function is recreated each time the component re-renders.

Navigate to `src/components/Todo/Todos.js` and apply a React hook to memoize the `formatTodoText()` function so that it is only re-created when necessary.

In the Todo app, type in the todo input and notice that the `<TodoItem>` components are no longer re-rendering on every keypress.

You've successfully prevented React from re-rendering a large list of items when their props have not changed!

## Task group 5: Split a module into a chunk

Now it's time to see where we can split up this app's main `bundle.js` file that it sends to users when they first load the page.

In your browser's developer tools, navigate to the Network tab, then reload the page.

Take note of the size of `bundle.js`.

Note: You may need to hard reload the page to see accurate data. Right-click on the reload icon in your browser, then click "Hard Reload".

Open the `src/components/Profile/getIconOptions.js` file. This file has a large amount of text appended to it to simulate a large module, and we'd like to load it as a separate chunk.

Navigate to `src/components/Profile/Profile.js` file. Inside it, `getIconOptions.js` is imported at the top of the file. Then, its exported function, `getIconOptions()`, is called inside a `useEffect()` hook.

Use your knowledge of code splitting to split this module into its own chunk when the component mounts.

Open the Network tab in the developer tools and hard reload the page.

Identify the file you just split into its own chunk, then take note of the size of `bundle.js`.

## Task group 6: Split a component into a chunk

When a user completes a todo item in this app, the app shows a shower of confetti. This effect is not shown to the user until they interact with the app, which makes it a good component to load lazily.

Navigate to `src/components/Todos/Todos.js` and update the `import` of `./Confetti.js` to load lazily.

When the app reloads, it should display an error state. The error message reads:

> A React component suspended while rendering, but no fallback UI was specified. Add a <Suspense fallback=...> component higher in the tree to provide a loading indicator or placeholder to display.

In `src/components/Todos/Todos.js`, wrap the rendered `<Confetti>` component with React's `<Suspense>` component. Make sure to provide it with a `fallback` prop for its loading state.

Note: We provided a `<Loader>` component in `src/components/Todos/Loader.js` if you'd like to import it and use it.

Open the Network tab in the developer tools and hard reload the app, then identify the new chunks that were split off from the `bundle.js` file.

Note: There are two new chunks. The configuration of webpack in create-react-app automatically splits modules into their own chunks when splitting components into a chunk.

## Conclusion

Great work! Visit our forums to compare your project to our sample solution code. You can also learn how to host your own solution on GitHub so you can share it with other learners! Your solution might look different from ours, and that’s okay! There are multiple ways to solve these projects, and you’ll learn more by seeing others’ code.
