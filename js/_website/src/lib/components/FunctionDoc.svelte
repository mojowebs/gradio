<script lang="ts">
	export let fn: any;
	import anchor from "../assets/img/anchor.svg";
	import { style_formatted_text } from "$lib/text";
</script>

<!-- name, signature, description, params -->

<div class="obj">
	<div class="flex flex-row items-center justify-between">
		<h3
			class="group text-3xl font-light py-4"
			id="{fn.parent
				.toLowerCase()
				.replace('gradio.', '')}-{fn.name.toLowerCase()}"
		>
			{fn.name}
			<a
				href="#{fn.parent
					.toLowerCase()
					.replace('gradio.', '')}-{fn.name.toLowerCase()}"
				class="invisible group-hover-visible"
				><img class="anchor-img" src={anchor} /></a
			>
		</h3>
	</div>

	{#if fn.override_signature}
		<div class="codeblock">
			<pre><code class="code language-python">{fn.override_signature}</code
				></pre>
		</div>
	{:else}
		<div class="codeblock">
			<pre><code class="code language-python"
					>{fn.parent}.<span>{fn.name}&lpar;</span
					><!--
        -->{#each fn.parameters as param}<!--
        -->{#if !("kwargs" in param) && !("default" in param) && param.name != "self"}<!--
            -->{param.name}, <!--
        -->{/if}<!--
        -->{/each}<!--  
        -->···<span
						>&rpar;</span
					></code
				></pre>
		</div>
	{/if}

	<h4
		class="mt-8 text-xl text-orange-500 font-light group"
		id="{fn.name.toLowerCase()}-description"
	>
		Description
		<a
			href="#{fn.name.toLowerCase()}-description"
			class="invisible group-hover-visible"
			><img class="anchor-img-small" src={anchor} /></a
		>
	</h4>
	<p class="mb-2 text-lg">{@html fn.description}</p>

	{#if fn.example}
		<h4
			class="mt-4 text-xl text-orange-500 font-light group"
			id="{fn.name.toLowerCase()}-example-usage"
		>
			Example Usage
			<a
				href="#{fn.name.toLowerCase()}-example-usage"
				class="invisible group-hover-visible"
				><img class="anchor-img-small" src={anchor} /></a
			>
		</h4>
		<div class="codeblock">
			<pre><code class="code language-python">{@html fn.example}</code></pre>
		</div>
	{/if}

	{#if (fn.parameters.length > 0 && fn.parameters[0].name != "self") || fn.parameters.length > 1}
		<h4
			class="mt-6 text-xl text-orange-500 font-light group"
			id="{fn.name.toLowerCase()}-arguments"
		>
			Arguments
			<a
				href="#{fn.name.toLowerCase()}-arguments"
				class="invisible group-hover-visible"
				><img class="anchor-img-small" src={anchor} /></a
			>
		</h4>

		<table class="table-fixed w-full leading-loose">
			<thead class="text-left">
				<tr>
					<th class="px-3 pb-3 font-semibold text-gray-700 w-2/5">Parameter</th>
					<th class="px-3 pb-3 font-semibold text-gray-700">Description</th>
				</tr>
			</thead>
			<tbody
				class=" rounded-lg bg-gray-50 border border-gray-100 overflow-hidden text-left align-top divide-y"
			>
				{#each fn.parameters as param}
					{#if param["name"] != "self"}
						<tr class="group hover:bg-gray-200/60 odd:bg-gray-100/80">
							<td class="p-3 w-2/5 break-words">
								<code class="block">
									{param["name"]}
								</code>
								<p class="text-gray-500 italic">
									{param["annotation"].replace("Sequence[", "list[")}
								</p>
								{#if "default" in param}
									<p class="text-gray-500 font-semibold">
										default: {param["default"]}
									</p>
								{:else if !("kwargs" in param)}
									<p class="text-orange-600 font-semibold italic">required</p>
								{/if}
							</td>
							<td class="p-3 text-gray-700 break-words">
								<p>{@html param["doc"] || ""}</p>
							</td>
						</tr>
					{/if}
				{/each}
			</tbody>
		</table>
	{/if}
</div>
