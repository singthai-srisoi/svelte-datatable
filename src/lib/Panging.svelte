<script>
    export let data = []
    export let columns = data.length ? Object.keys(data[0]) : []

    let default_data = data

    let page_size = 5,
        current_page = 1,
        total_pages = Math.ceil(data.length / page_size),
        pages = Array.from({length: total_pages}, (_, i) => i + 1),
        paginated_data = data.slice(0, page_size)

    console.table(
        {
            page_size,
            current_page,
            total_pages,
            pages,
            paginated_data
        }
    )

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
    <select name="page" bind:value={current_page}>
        {#each pages as page}
            <option value={page}>{page}</option>
        {/each}
    </select>
    <button on:click={() => current_page = (current_page > 1) ? (current_page - 1) : 1}>-</button>
    <button on:click={() => current_page = (current_page < pages.length) ?  (current_page + 1) : pages.length}>+</button>
</div>

