<script lang="ts">
	import { LinkPreview } from 'bits-ui';
	import TimezoneCard from './TimezoneCard.svelte';
	import { scale } from 'svelte/transition';
	import type { Snippet } from 'svelte';

	type Variant = 'default' | 'muted' | 'ghost';
	type Side = 'top' | 'right' | 'bottom' | 'left';
	type Align = 'start' | 'center' | 'end';

	let {
		date,
		timezones = ['UTC'] as string[],
		updateInterval = 1000,
		variant = 'default' as const,
		side = 'top' as const,
		align = 'center' as const,
		sideOffset = 4,
		alignOffset = 0,
		openDelay = 500,
		closeDelay = 300,
		trigger
	}: {
		date: Date | string | number;
		timezones?: string[];
		updateInterval?: number;
		variant?: Variant;
		side?: Side;
		align?: Align;
		sideOffset?: number;
		alignOffset?: number;
		openDelay?: number;
		closeDelay?: number;
		trigger?: (props: { dateObj: Date }) => ReturnType<Snippet>;
	} = $props();

	const scaleStart = 0.95;
	const scaleDuration = 200;

	const transformOrigins: Record<Side, string> = {
		top: 'bottom center',
		right: 'left center',
		bottom: 'top center',
		left: 'right center'
	} as const;

	const dateObj = $derived(date instanceof Date ? date : new Date(date));
	const transformOrigin = $derived(transformOrigins[side] ?? 'center center');

	const dateFormatter = new Intl.DateTimeFormat('en-US', {
		month: 'short',
		day: 'numeric',
		year: 'numeric',
		hour: '2-digit',
		minute: '2-digit'
	});

	const formatRelativeTime = $derived((date: Date): string => {
		const now = new Date();
		const diff = now.getTime() - date.getTime();
		const seconds = Math.floor(diff / 1000);
		const minutes = Math.floor(seconds / 60);
		const hours = Math.floor(minutes / 60);
		const days = Math.floor(hours / 24);

		if (seconds < 5) return 'just now';
		if (seconds < 60) return `${seconds} seconds ago`;
		if (minutes < 60) return `${minutes} minutes ${seconds % 60} seconds ago`;
		if (hours < 24) return `${hours} hours ago`;
		if (days < 7) return `${days} days ago`;

		return date.toLocaleDateString();
	});

	let formattedTime = $state('');

	$effect(() => {
		formattedTime = formatRelativeTime(dateObj);
		const timer = setInterval(() => {
			formattedTime = formatRelativeTime(dateObj);
		}, updateInterval);

		return () => clearInterval(timer);
	});

	let isOpen = $state(false);

	function handleFocus() {
		isOpen = true;
	}

	function handleBlur() {
		isOpen = false;
	}
</script>

<LinkPreview.Root {openDelay} {closeDelay} open={isOpen} onOpenChange={(open) => (isOpen = open)}>
	<div
		class="trigger"
		class:variant-default={variant === 'default'}
		class:variant-muted={variant === 'muted'}
		class:variant-ghost={variant === 'ghost'}
		tabindex="0"
		role="button"
		aria-label="Show relative time"
		onfocus={handleFocus}
		onblur={handleBlur}
	>
		<LinkPreview.Trigger>
			{#if trigger}
				{@render trigger({ dateObj })}
			{:else}
				<time dateTime={dateObj.toISOString()}>
					{dateFormatter.format(dateObj)}
				</time>
			{/if}
		</LinkPreview.Trigger>
	</div>

	<LinkPreview.Content {side} {align} {sideOffset} {alignOffset} class="content">
		{#snippet child({ open, props, wrapperProps })}
			{#if open}
				<div {...wrapperProps}>
					<div
						{...props}
						style="transform-origin: {transformOrigin}"
						transition:scale={{
							duration: scaleDuration,
							start: scaleStart
						}}
					>
						<time dateTime={dateObj.toISOString()} class="relative-time">
							{formattedTime}
						</time>
						<div role="list" class="timezone-list">
							{#each timezones as timezone}
								<TimezoneCard date={dateObj} {timezone} />
							{/each}
							<TimezoneCard date={dateObj} />
						</div>
					</div>
				</div>
			{/if}
		{/snippet}
	</LinkPreview.Content>
</LinkPreview.Root>

<style>
	.trigger {
		display: inline-flex;
		width: fit-content;
		align-items: center;
		justify-content: center;
		font-size: 0.875rem;
		transition: color 0.2s;
		cursor: default;
		outline: none;
	}

	.trigger:focus-visible {
		outline: 2px solid var(--ring);
		outline-offset: 2px;
	}

	.variant-default {
		color: var(--foreground);
		opacity: 0.7;
	}

	.variant-default:hover {
		opacity: 0.9;
	}

	.variant-muted {
		color: var(--foreground);
		opacity: 0.5;
	}

	.variant-muted:hover {
		opacity: 0.7;
	}

	.variant-ghost {
		color: var(--foreground);
		opacity: 0.7;
	}

	.variant-ghost:hover {
		text-decoration: underline;
	}

	.content {
		z-index: 50;
		display: flex;
		width: 100%;
		max-width: 420px;
		flex-direction: column;
		gap: 0.5rem;
		border-radius: var(--radius);
		border: 1px solid var(--border);
		background-color: var(--popover);
		padding: 0.75rem;
		color: var(--popover-foreground);
		box-shadow:
			0 4px 6px -1px rgb(0 0 0 / 0.1),
			0 2px 4px -2px rgb(0 0 0 / 0.1);
		outline: none;
	}

	.relative-time {
		color: var(--muted-foreground);
		font-size: 0.875rem;
	}

	.timezone-list {
		display: flex;
		flex-direction: column;
		gap: 0.25rem;
	}
</style>
