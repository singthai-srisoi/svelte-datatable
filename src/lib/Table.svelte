<script>
    import Panging from "./Panging.svelte";

    export let data = []
    export let columns = data.length ? Object.keys(data[0]) : []

    let default_data = data
    let page_size = 5,
        current_page = 1,
        total_pages = Math.ceil(data.length / page_size),
        pages = Array.from({length: total_pages}, (_, i) => i + 1)

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

    $:{
        total_pages = Math.ceil(data.length / page_size)
        pages = Array.from({length: total_pages}, (_, i) => i + 1)

        if (current_page > total_pages) {
            current_page = 1
        }
    }

</script>

<div>
    <input type="text" name="search" placeholder="search" on:input={handle_search}/>
</div>

<Panging {data} {columns} bind:page_size={page_size} bind:current_page={current_page} {total_pages} {pages}/>

