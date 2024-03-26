<script>
    export let data = []
    export let columns = data.length ? Object.keys(data[0]) : []

    let default_data = data

    export let page_size = 5
    export let current_page = 1
    export let total_pages = Math.ceil(data.length / page_size)
    export let pages = Array.from({length: total_pages}, (_, i) => i + 1)
    let paginated_data = data.slice(0, page_size)

    $:{
        let start = (current_page - 1) * page_size
        let end = start + page_size
        paginated_data = data.slice(start, end)
    }

</script>

<table>
    <thead>
        <tr>
            {#each columns as column}
                <th>{column}</th>
            {/each}
        </tr>
    </thead>
    <tbody>
        {#each paginated_data as row}
            <tr>
                {#each columns as column}
                    <td>{row[column]}</td>
                {/each}
            </tr>
        {/each}
    </tbody>
</table>

<div>
    <select name="page_size" bind:value={page_size}>
        <option value="5">5</option>
        <option value="10">10</option>
        <option value="15">15</option>
        <option value="20">20</option>
    </select>
    <select name="page" bind:value={current_page}>
        {#each pages as page}
            <option value={page}>{page}</option>
        {/each}
    </select>
    <button on:click={() => current_page = (current_page > 1) ? (current_page - 1) : 1}
        disabled={current_page === 1}>-</button>
    <button on:click={() => current_page = (current_page < pages.length) ?  (current_page + 1) : pages.length}
        disabled={current_page === pages.length}>+</button>
</div>

