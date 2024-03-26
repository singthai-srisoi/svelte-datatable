<script>
    import Panging from "./Panging.svelte";

    export let data = []
    export let columns = data.length ? Object.keys(data[0]) : []

    let default_data = data

    let search_pane = columns.map(key => {
        let values = data.map(row => row[key])
        values = [...new Set(values)]
        return {key, values}
    })

    let handle_search = (e) => {
        let search = e.target.value.toLowerCase()
        if (!search) {
            data = default_data
            return
        }
        let filtered_data = default_data.filter(row => {
            return Object.values(row).join('').toLocaleLowerCase().includes(search)
        })
        data = filtered_data
    } 

    let handle_select = (e) => {
        let key = e.target.dataset.key
        // get value in list
        let value = Array.from(e.target.selectedOptions).map(option => option.value)
        let values = value.join('')
        if (!value || values.length === 0) {
            data = default_data
            return
        }
        let filtered_data = default_data.filter(row => {
            return values.includes(row[key])
        })
        data = filtered_data
        
    }
    
    // paginnation stuff
    let page_size = 5,
        current_page = 1,
        total_pages = Math.ceil(data.length / page_size),
        pages = Array.from({length: total_pages}, (_, i) => i + 1)
    $:{
        total_pages = Math.ceil(data.length / page_size)
        pages = Array.from({length: total_pages}, (_, i) => i + 1)

        if (current_page > total_pages) {
            current_page = 1
        }
    }

    

</script>

<div id="seacrch-pane">
    {#each search_pane as keys}
    <div>
        <label for={keys.key}>{keys.key}</label>
        <select name={keys.key} data-key={keys.key} multiple on:change={handle_select}>
            <option value="">All</option>
            {#each keys.values as value}
            <option value={value}>{value}</option>
            {/each}
        </select>
        <button>clear</button>
    </div>        
    {/each}
</div>
<div>
    <input type="text" name="search" placeholder="search" on:input={handle_search}/>
</div>

<Panging {data} {columns} bind:page_size={page_size} bind:current_page={current_page} {total_pages} {pages}/>

<style>    

    select {
        padding: 5px;
        margin: 5px;
    }

    button {
        padding: 5px;
        margin: 5px;
    }

    #seacrch-pane {
        display: flex;
        justify-content: space-between;
    }

    #seacrch-pane div {
        margin: 5px;
    }

    #seacrch-pane select {
        width: 100px;
    }

    #seacrch-pane button {
        padding: 5px;
        margin: 5px;
    }
</style>

