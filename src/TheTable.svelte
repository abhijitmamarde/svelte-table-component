<script>
    import { onMount } from "svelte";
    export let name = "Table";
    export let api_route;

    let recs = [];
    let cols = [];
    let display_cols = [];
    let sort_by = "";

    $:recs_length = recs.length;

    onMount(async () => {
        fetch_records("");
    });

    function fetch_records(new_sort_by) {
        var req_data = {
            sort_by: new_sort_by
        };
        fetch(api_route, {
            mode: 'cors',
            headers: {
                'Access-Control-Allow-Origin':'*'
            },
            method: 'POST',
            body: JSON.stringify(req_data),
        })
        .then(response => response.json())
        .then(data => {
                console.log(data);
                recs = data["data"];
                cols = data["columns"];
                display_cols = data["display_columns"];
                sort_by = data["sort_by"];

        }).catch(error => {
            console.log(error);
            return [];
        });
    }

    function on_click_header(col) {
        console.log("clicked header: " + col)
        if (sort_by == col) {
            fetch_records("-" + col)
        } else if (sort_by == "-" + col) {
            fetch_records(col)
        } else {
            fetch_records(col)
        }
    }
</script>
<!-- 
<p>Name: {name}</p>
<p>Route: {api_route}</p>
<p>Records: {recs.length}</p> -->

<div class="table-container">
<table class="table is-fullwidth">
    <caption>{name}</caption>
    <thead>        
        {#each cols as col}
            <td on:click="{()=>on_click_header(col)}">{display_cols[col]}
                {#if sort_by == col}
                    <span>&uarr;</span>
                {:else if sort_by == "-" + col}
                    <span>&darr;</span>
                {/if}
            </td>
        {/each}
    </thead>

	{#each recs as rec}
        <tr>
        {#each cols as col}
            <td>{rec[col]}</td>
        {/each}
        </tr>
	{/each}
</table>
</div>