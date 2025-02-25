<script lang="ts">
	import type { ComponentType, SvelteComponent } from 'svelte';
	import * as Popover from '$lib/components/ui/popover/index.js';
	import { createEventDispatcher } from 'svelte';
	import { Label } from '@/components/ui/label';
	import { toast } from 'svelte-sonner';

	export let icon: ComponentType<SvelteComponent>;
	export let text: string | undefined = undefined;
	export let strokeWidth: number = 2.5;
	export let className: string = '';
	export let colorOnClick = false;
	export let outline = true;

	export let isActive = false;
	export let popover: string | null = null;
	export let animate = false;

	let opened = false;

	function handleClick(event: MouseEvent) {
		event.preventDefault();
		event.stopPropagation();

		opened = !opened;

		if (colorOnClick) {
			isActive = !isActive;
		}

		dispatch('click', event);
	}

	const dispatch = createEventDispatcher();
</script>

{#if popover}
	<Popover.Root bind:open={opened} portal={null}>
		<Popover.Trigger asChild let:builder>
			<button
				{...builder}
				class:active={isActive}
				class:animate
				on:click={handleClick}
				class="shit {outline
					? 'border-2 border-solid border-primary p-1.5'
					: ''} inline-flex items-center justify-center rounded-xl font-bold text-primary {className}"
			>
				<svelte:component this={icon} {strokeWidth} class="h-6 w-6 {text ? 'mr-1' : ''}" />
				{#if text}
					<span>{text}</span>
				{/if}
			</button>
		</Popover.Trigger>
		<Popover.Content class="w-60">
			<div class="flex items-center justify-center gap-2">
				<svelte:component this={icon} {strokeWidth} class="h-12 w-12" />
				<Label>{popover}</Label>
			</div>
		</Popover.Content>
	</Popover.Root>
{:else}
	<button
		class:active={isActive}
		class:animate
		on:click={handleClick}
		class="shit {outline
			? 'border-2 border-solid border-primary p-1.5'
			: ''} inline-flex items-center justify-center rounded-xl font-bold text-primary {className}"
	>
		<svelte:component this={icon} {strokeWidth} class="h-6 w-6 {text ? 'mr-1' : ''}" />
		{#if text}
			<span>{text}</span>
		{/if}
	</button>
{/if}

<style>
	.shit {
		transition:
			filter 0.2s ease-in-out,
			border 0.2s ease-in-out,
			transform 0.2s ease-in-out,
			background-color 0.1s ease-in-out,
			color 0.1s ease-in-out;
	}

	.shit:hover {
		filter: drop-shadow(0 0px 20px hsl(var(--primary) / 0.8));
		transform: rotate(3deg);
	}

	.shit.active {
		background-color: hsl(var(--primary));
		color: hsl(var(--background) / var(--tw-text-opacity));
		transform: rotate(0deg);
	}

	@keyframes popIn {
		0% {
			transform: scale(1) rotate(0deg);
		}
		50% {
			transform: scale(1.2) rotate(3deg);
		}
		100% {
			transform: scale(1) rotate(0deg);
		}
	}

	.animate {
		animation: popIn 0.3s ease-in-out;
	}
</style>
