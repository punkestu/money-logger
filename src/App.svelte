<script>
	import { onMount } from "svelte";

	let date = new Date().toISOString().split("T")[0];
	let debit = 0;
	let kredit = 0;
	let desc = "";
	let ledger = [];

	onMount(() => {
		ledger = JSON.parse(localStorage.getItem("ledger")) || [];
	});

	function add() {
		ledger = [...ledger, { date, debit, kredit, desc }];
		localStorage.setItem("ledger", JSON.stringify(ledger));
		date = new Date().toISOString().split("T")[0];
		debit = 0;
		kredit = 0;
		desc = "";
	}

	function remove(index) {
		ledger = ledger.filter((_, i) => i !== index);
		localStorage.setItem("ledger", JSON.stringify(ledger));
	}
</script>

<form
	on:submit={(e) => {
		e.preventDefault();
		add();
	}}
	class="flex flex-col-reverse md:flex-row p-4 gap-4"
>
	<textarea
		bind:value={desc}
		class="px-2 py-1 resize-none w-full bg-none border-b-2"
		placeholder="Deskripsi..."
	></textarea>
	<aside class="flex gap-4">
		<div>
			<input
				type="date"
				bind:value={date}
				class="px-2 w-full self-end text-right bg-none border-b-2"
			/>
			<label class="flex gap-2 items-end my-1">
				<span class="w-1/4">Kredit</span>
				<input type="number" bind:value={kredit} class="px-2 w-full bg-none border-b-2" />
			</label>
			<label class="flex gap-2 items-end my-1">
				<span class="w-1/4">Debit</span>
				<input type="number" bind:value={debit} class="px-2 w-full bg-none border-b-2" />
			</label>
		</div>
		<button
			type="submit"
			class="bg-slate-500 text-slate-50 hover:bg-slate-600 hover:text-slate-100 duration-300 px-4"
			>Add</button
		>
	</aside>
</form>

<section class="p-4 w-screen overflow-x-auto">
	<div class="w-[200vw] md:w-full">
		<table class="w-full table-auto">
			<tr>
				<th class="px-2 text-left border-2 border-slate-600">Date</th>
				<th class="px-2 text-right border-2 border-slate-600">Kredit</th>
				<th class="px-2 text-right border-2 border-slate-600">Debit</th>
				<th class="px-2 text-left border-2 border-slate-600">Description</th>
				<th class="px-2 text-center border-2 border-slate-600">Action</th>
			</tr>
			{#each ledger as row, index}
				<tr class="border-b">
					<td class="px-2 text-left">{row.date}</td>
					<td class="px-2 text-right">{row.kredit}</td>
					<td class="px-2 text-right">{row.debit}</td>
					<td class="px-2 text-left">{row.desc || "-"}</td>
					<td class="px-2 text-center"><button on:click={() => remove(index)}>Delete</button></td>
				</tr>
			{/each}
		</table>
	</div>
</section>
