<script lang="ts">
	import '../theme.postcss';
	import '@skeletonlabs/skeleton/styles/all.css';
	import '../app.postcss';
	import { AppShell, AppBar, LightSwitch } from '@skeletonlabs/skeleton';
	import logo from '$lib/assets/logo.png';
	import { fly } from 'svelte/transition';
	import { backOut } from 'svelte/easing';
	import { browser } from '$app/environment';
	import Dropdown from '$lib/components/Dropdown.svelte';

	let logoHovered = false;
	let isMobile: boolean;
	if (browser) {
		isMobile = window.matchMedia('(max-width: 768px)').matches;
	}

	let dropdownContent = {
		name: 'Estrutura',
		items: [
			{
				name: 'Laboratórios',
				link: '#'
			},
			{
				name: 'Biblioteca',
				link: '#'
			},
			{
				name: 'Área de testes',
				link: '#'
			}
		]
	};

	const menuItems = [
		{
			name: 'Quem somos',
			link: '/about'
		},
		{
			name: 'Equipe',
			link: '/team'
		}
	];
</script>

{#if isMobile}
	<AppBar />
{:else}
	<AppShell>
		<svelte:fragment slot="header">
			<AppBar padding="p-0">
				<svelte:fragment slot="lead">
					<img
						src={logo}
						alt="GEDAE"
						class="z-10"
						on:mouseenter={() => {
							logoHovered = true;
						}}
						on:mouseleave={() => {
							logoHovered = false;
						}}
					/>
					{#if logoHovered}
						<div
							class="border-l-4 border-white font-bold select-none absolute translate-x-28"
							transition:fly={{ duration: 550, x: -90, easing: backOut }}
						>
							<div class="bg-red-500 text-white w-fit h-[29px] p-1">GRUPO DE ESTUDOS E</div>
							<div class="bg-red-500 text-white w-fit h-[30px] p-1">DESENVOLVIMENTO DE</div>
							<div class="bg-red-500 text-white w-fit h-[30px] p-1">ALTERNATIVAS ENERGÉTICAS</div>
						</div>
					{/if}
				</svelte:fragment>
				<div class="h-[90px] flex flex-row min-w-full">
					{#each menuItems as item}
						<a
							href={item.link}
							class="flex-grow btn variant-filled-surface border-secondary-700 border-b-2 border-l-2"
						>
							{item.name}
						</a>
					{/each}
					<Dropdown {dropdownContent} />
				</div>
			</AppBar>
		</svelte:fragment>
		<svelte:fragment slot="sidebarLeft">Sidebar Left</svelte:fragment>
		<svelte:fragment slot="sidebarRight">Sidebar Right</svelte:fragment>
		<svelte:fragment slot="pageHeader">Page Header</svelte:fragment>
		<!-- Router Slot -->
		<slot />
		<!-- ---- / ---- -->
		<svelte:fragment slot="pageFooter">Page Footer</svelte:fragment>
		<svelte:fragment slot="footer"><LightSwitch /></svelte:fragment>
	</AppShell>
{/if}
