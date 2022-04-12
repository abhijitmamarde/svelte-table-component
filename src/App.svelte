<script>
	import TheTable from "./TheTable.svelte";
	export let name;
</script>

<section class="section">
    <div class="container">
      <h1 class="title">
        Hello {name}!
      </h1>	  
      <p class="subtitle">
      Demo of svelte component for table data - <strong>TheTable</strong>
      </p>
    </div>
</section>

<section>
	<TheTable name="Users" api_route="http://localhost:5000/api/users"/>
	<TheTable name="Customers" api_route="http://localhost:5000/api/customers"/>
</section>