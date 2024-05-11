# Svelte 5 Runes Todo

A SvelteKit application that implements a simple todo list with advanced features using the new Runes APIs in Svelte 5.

## Features

- Add, delete, and toggle tasks.
- Categorize tasks with tags.
- Reactive state management.
- Persistence with local storage.
- Undo/redo functionality.
- Task statistics and filtered views.

## Files

- `todo-list.svelte`: Main application logic.
- `task-item.svelte`: Single task component.
- `$lib`: Contains Task type definition.

## Code Overview

The `todo-list.svelte` file uses several features of Svelte 5:

- `$state`: This feature is used to create reactive states for tasks, input value, and tags. Any changes to these states will automatically cause the component to re-render.
- `frozen`: This feature is used to prevent reactivity for tags. Changes to tags will not cause the component to re-render.
- `$derived.by`: This feature is used to create derived states for total tasks, completed tasks, and filtered tasks. These states automatically update whenever their dependencies change.
- `$effect`: This feature is used to handle side effects, such as saving and loading tasks from local storage.
- `$inspect`: This feature is used for debugging. It can log the current state of tasks to the console.

Note: These new Runes APIs are part of Svelte 5.

## Running the Application

This application requires Bun or Node.js. Follow these steps to get it up and running:

1. Clone the repository to your local machine.
2. Navigate to the project directory.
3. Install the necessary dependencies using `bun install` or `npm install`.
4. Start the server using `bun run dev` or `npm run dev`.

For optimal development experience with Svelte syntax, consider installing the Svelte extension for your IDE.
