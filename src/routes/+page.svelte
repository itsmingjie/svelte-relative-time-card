<script lang="ts">
	import Clock from '@lucide/svelte/icons/clock';
	import { RelativeTimeCard } from '$lib/index.js';

	import CopyIcon from '@lucide/svelte/icons/copy';
	import CheckIcon from '@lucide/svelte/icons/check';

	const now = $state(new Date());
	const fiveMinutesAgo = $derived(new Date(now.getTime() - 5 * 60 * 1000));
	const oneHourAgo = $derived(new Date(now.getTime() - 60 * 60 * 1000));
	const oneDayAgo = $derived(new Date(now.getTime() - 24 * 60 * 60 * 1000));

	let copied = $state(false);
	const COPY_TIMEOUT = 2000;

	function copyToClipboard() {
		navigator.clipboard.writeText('npm install svelte-relative-time-card');
		copied = true;
		setTimeout(() => {
			copied = false;
		}, COPY_TIMEOUT);
	}
</script>

<svelte:head>
	<title>Relative Time Card</title>
</svelte:head>

<div class="container">
	<div class="header">
		<h1 class="title">Relative Time Card</h1>
		<p class="description">
			A hover card that displays relative time relative to local time with timezone information.
			Built for Svelte 5.
		</p>
	</div>

	<div class="section">
		<h2 class="section-title">Installation</h2>
		<div class="code-block">
			<code>npm install svelte-relative-time-card</code>
			<button class="copy-button" onclick={copyToClipboard}>
				<span class="sr-only">Copy</span>
				{#if copied}
					<CheckIcon class="copy-icon" size={16} />
				{:else}
					<CopyIcon class="copy-icon" size={16} />
				{/if}
			</button>
		</div>
	</div>

	<div class="demo-section">
		<div class="demo-item">
			<span class="demo-label">Basic usage</span>
			<RelativeTimeCard date={fiveMinutesAgo} />
		</div>
		<div class="demo-item">
			<span class="demo-label">Different variants</span>
			<div class="variants-container">
				<RelativeTimeCard date={oneHourAgo} variant="default" />
				<RelativeTimeCard date={oneHourAgo} variant="muted" />
				<RelativeTimeCard date={oneHourAgo} variant="ghost" />
			</div>
		</div>
		<div class="demo-item">
			<span class="demo-label">Multiple timezones</span>
			<RelativeTimeCard
				date={oneDayAgo}
				timezones={['America/New_York', 'Europe/London', 'Asia/Tokyo']}
			/>
		</div>
		<div class="demo-item">
			<span class="demo-label">Custom trigger</span>
			<RelativeTimeCard date={now}>
				{#snippet trigger()}
					<button class="custom-trigger">
						<Clock class="icon" size={16} />
						View time details
					</button>
				{/snippet}
			</RelativeTimeCard>
		</div>
		<div class="demo-item">
			<span class="demo-label">Different positions</span>
			<div class="positions-container">
				<RelativeTimeCard date={now} side="top" align="start">
					{#snippet trigger({ dateObj })}
						Top Start
					{/snippet}
				</RelativeTimeCard>
				<RelativeTimeCard date={now} side="right" align="center">
					{#snippet trigger({ dateObj })}
						Right Center
					{/snippet}
				</RelativeTimeCard>
				<RelativeTimeCard date={now} side="bottom" align="end">
					{#snippet trigger({ dateObj })}
						Bottom End
					{/snippet}
				</RelativeTimeCard>
			</div>
		</div>
	</div>

	<div class="footer">
		<div class="footer-text">
			Built by <a href="https://github.com/itsmingjie" class="link" target="_blank">@itsmingjie</a>,
			based on
			<a href="https://github.com/sadmann7" class="link" target="_blank">sadmann7</a>'s React
			<a
				href="https://www.diceui.com/docs/components/relative-time-card"
				class="link"
				target="_blank">implementation</a
			>.
		</div>
	</div>
</div>

<style>
	.header {
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
		border-bottom: 1px solid var(--border);
		padding-bottom: 1rem;
	}

	.title {
		font-size: 1.5rem;
		font-weight: 700;
		margin: 0;
	}

	.description {
		color: var(--muted-foreground);
		margin: 0;
		font-size: 0.875rem;
		line-height: 1.5;
	}

	.container {
		display: flex;
		flex-direction: column;
		gap: 2rem;
	}
	.demo-section {
		display: flex;
		flex-direction: column;
		gap: 1.5rem;
	}

	.demo-item {
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
	}

	.demo-label {
		color: var(--muted-foreground);
		font-size: 0.875rem;
	}

	.variants-container {
		display: flex;
		gap: 1rem;
		flex-direction: column;
		align-items: flex-start;
	}

	@media (min-width: 1024px) {
		.variants-container {
			flex-direction: row;
			align-items: center;
		}
	}

	.positions-container {
		display: flex;
		align-items: center;
		gap: 1rem;
	}

	.custom-trigger {
		display: inline-flex;
		align-items: center;
		gap: 0.5rem;
		border-radius: var(--radius);
		border: 1px solid var(--input);
		background-color: var(--background);
		padding: 0.25rem 0.75rem;
		font-size: 0.875rem;
		font-weight: 500;
		transition:
			background-color 0.2s,
			color 0.2s;
	}

	.custom-trigger:hover {
		background-color: var(--accent);
		color: var(--accent-foreground);
	}

	.custom-trigger:focus-visible {
		outline: none;
		box-shadow:
			0 0 0 2px var(--ring),
			0 0 0 4px var(--background);
	}

	.footer {
		border-top: 1px solid var(--border);
		padding-top: 1rem;
	}

	.footer-text {
		font-size: 0.875rem;
	}

	.link {
		text-decoration: underline;
	}

	.section {
		display: flex;
		flex-direction: column;
		gap: 1rem;
	}

	.section-title {
		font-size: 1.25rem;
		font-weight: 600;
		margin: 0;
	}

	.code-block {
		display: flex;
		align-items: center;
		justify-content: space-between;
		background-color: var(--accent);
		padding: 0.75rem 1rem;
		border-radius: var(--radius);
		font-family:
			ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, 'Liberation Mono', 'Courier New',
			monospace;
		font-size: 0.875rem;
	}

	.copy-button {
		display: inline-flex;
		align-items: center;
		justify-content: center;
		padding: 0.25rem;
		border-radius: var(--radius);
		color: var(--muted-foreground);
		transition: color 0.2s;
		cursor: pointer;
		background: none;
		border: none;
	}

	.copy-button:hover {
		color: var(--foreground);
	}

	.sr-only {
		position: absolute;
		width: 1px;
		height: 1px;
		padding: 0;
		margin: -1px;
		overflow: hidden;
		clip: rect(0, 0, 0, 0);
		white-space: nowrap;
		border-width: 0;
	}
</style>
