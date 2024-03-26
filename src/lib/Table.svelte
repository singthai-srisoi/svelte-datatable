<script>
    import Panging from "./Panging.svelte";

    export let data = []
    export let columns = data.length ? Object.keys(data[0]) : []

    let default_data = data

    let handle_search = (e) => {
        let search = e.target.value.toLowerCase()
        if (!search) {
            data = default_data
            return
        }
        let filtered_data = data.filter(row => {
            return Object.values(row).join('').toLocaleLowerCase().includes(search)
        })
        data = filtered_data
    }

</script>

<div>
    <input type="text" name="search" placeholder="search" on:input={handle_search}/>
</div>

<Panging {data} {columns} />

