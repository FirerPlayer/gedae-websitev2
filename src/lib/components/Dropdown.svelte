<script lang="ts">
	import { onMount } from 'svelte';
	import { expoOut } from 'svelte/easing';
	import tippy from 'tippy.js';
	import 'tippy.js/animations/shift-away-extreme.css';
	let menu: Element;
	let btn: Element;
	let opened = false;
	export let lass = '';

	onMount(() => {
		tippy(btn, {
			content: menu,
			interactive: true,
			interactiveDebounce: 100,
			placement: 'bottom-end',
			offset: [0, 0],
			onShow: (instance) => {
				opened = true;
			},
			onHide(instance) {
				opened = false;
			}
		});
	});

	function growDown(node: Element, { duration = 200, delay = 0 }) {
		const h = Number(getComputedStyle(node).height.slice(0, -2));

		return {
			duration,
			delay,
			easing: expoOut,
			css: (t: number) => `height: ${h * t}px`
		};
	}

	interface DropdownContent {
		name: String;
		items: {
			name: string;
			link: string;
		}[];
	}
	export let dropdownContent: DropdownContent = {
		name: 'Exemplo',
		items: [
			{
				name: 'texto1',
				link: '#'
			},
			{
				name: 'texto2',
				link: '#'
			},
			{
				name: 'texto3',
				link: '#'
			}
		]
	};
</script>

<button {class} bind:this={btn} class="btn variant-filled">
	{dropdownContent.name}
</button>
<div bind:this={menu}>
	{#if opened}
		<nav transition:growDown={{}} class="box list-nav w-fit min-w-[200px] variant-filled">
			<ul>
				<li>
					{#each dropdownContent.items as item}
						<a href={item.link}>
							<span class="flex-auto">{item.name}</span>
						</a>
					{/each}
				</li>
			</ul>
		</nav>
	{/if}
</div>
