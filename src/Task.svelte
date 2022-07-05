<script>
    let promise = getListOfTasks();
    let data = [];

    async function getListOfTasks() {
        return await fetch('https://jsonplaceholder.typicode.com/posts')
            .then((response) => response.json())
            .then((_data) => {
                data = _data.map((item) => {
                    item.done = false;
                    return item;
                });
                return _data;
            });
    }

</script>

{#await promise}
    <p>loading...</p>
    <!--{:then tasks}-->
    {:catch error}
        <p style="color: red;">{error.message}</p>
{/await}

<ul>
    {#each data as { id, title, done }}
        <li>
            <input
                    type="checkbox"
                    bind:checked={done}
            />
            <p style="text-decoration: {done ? 'line-through' : 'auto'}">{id}. {title}</p>
        </li>
    {/each}
</ul>

<style>
    ul {
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    li {
        width: 500px;
        display: flex;
        justify-content: start;
        align-items: center;
        gap: 10px;
    }
    li p {
        width: 450px;
        text-overflow: ellipsis;
        overflow: hidden;
        white-space: nowrap;
        text-align: start;
    }
    input[type="checkbox"] {
        margin: 0;
    }
</style>