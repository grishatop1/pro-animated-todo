<script>
    import { flip } from "svelte/animate";
    import { scale } from "svelte/transition";
    import { expoIn, expoOut } from "svelte/easing";

    let todos = [
        { id: 0, done: false, content: "do homework" },
        { id: 1, done: false, content: "walk dog" },
        { id: 2, done: false, content: "make pizza" },
        { id: 3, done: false, content: "buy milk" },
    ];
    let uid = todos.length + 1;

    const remove = (todo) => {
        todos = todos.filter((t) => t !== todo);
    };

    function add(input) {
		const todo = {
			id: uid++,
			done: false,
			content: input.value
		};

		todos = [todo, ...todos];
		input.value = '';
	}

    const crossline = (node, { delay = 0, duration = 600, easing = expoOut }) => {
        return {
            delay,
            duration,
            easing,
            css: (t) => {
                return `
                width: ${100 * t}%;
            `;
            },
        };
    };
</script>

<h2>Cool ToDo List</h2>

<input type="text"
    on:keydown="{event => event.key === 'Enter' && add(event.target)}"
    placeholder="Add an item"
    style="margin-bottom: 20px"
>

{#each todos as todo (todo.id)}
    <div
        class="item"
        animate:flip={{ duration: 450 }}
        transition:scale
        style="display: flex; position: relative;"
    >
        <input type="checkbox" bind:checked={todo.done} style="margin-right: 10px;"/>

        <div style="position: relative;">
            <input
                type="text"
                placeholder="What needs to be done?"
                bind:value={todo.content}
                disabled={todo.done}
                style="opacity: {todo.done ? 0.2 : 1};"
            />
            {#if todo.done}
                <div
                    class="line-done"
                    transition:crossline|local
                   
                />
        {/if}
        </div>

        <button style="margin-left: 15px; background-color: {(todo.done ? '#ff4545' : '')}" on:click={() => remove(todo)}>
            <svg style="width: 10px; height: 20px; transition: fill 0.2s;" fill="{(todo.done ? '#ffffff' : '#000000')}" height="800px" width="800px" version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" 
                viewBox="0 0 460.775 460.775" xml:space="preserve">
            <path d="M285.08,230.397L456.218,59.27c6.076-6.077,6.076-15.911,0-21.986L423.511,4.565c-2.913-2.911-6.866-4.55-10.992-4.55
                c-4.127,0-8.08,1.639-10.993,4.55l-171.138,171.14L59.25,4.565c-2.913-2.911-6.866-4.55-10.993-4.55
                c-4.126,0-8.08,1.639-10.992,4.55L4.558,37.284c-6.077,6.075-6.077,15.909,0,21.986l171.138,171.128L4.575,401.505
                c-6.074,6.077-6.074,15.911,0,21.986l32.709,32.719c2.911,2.911,6.865,4.55,10.992,4.55c4.127,0,8.08-1.639,10.994-4.55
                l171.117-171.12l171.118,171.12c2.913,2.911,6.866,4.55,10.993,4.55c4.128,0,8.081-1.639,10.992-4.55l32.709-32.719
                c6.074-6.075,6.074-15.909,0-21.986L285.08,230.397z"/>
            </svg>
        </button>
    </div>
{/each}

<style>
    .item {
        margin: 10px;
        width: fit-content;
    }
    input[type="text"] {
        padding: 4px;
        transition: opacity 0.2s;
        border: 1px solid gray;
        user-select: none;
    }
    button {
        border: none;
        height: 30px;
        width: 30px;
        border-radius: 50%;
        line-height: 20px;
        display: flex;
        justify-content: center;
        align-items: center;
        transition: background-color 0.5s;
    }
    .line-done {
        position: absolute;
        height: 2px;
        border-radius: 100%;
        background-color: black;
        top: 50%;
        transition: all 0.5s;
        z-index: 20;
        width: 100%;
    }
</style>
