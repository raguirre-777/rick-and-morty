<script>
    export let name 
    export let image
</script>
<style>

</style>

<article class="contact-card">
	<h2>
		<slot>
			<span class="missing">{name}</span>
		</slot>
	</h2>

	<div class="address">
		<slot>
			
		</slot>
	</div>

	<div class="email">
		<slot>
			<img src={image}>
		</slot>
	</div>
</article>