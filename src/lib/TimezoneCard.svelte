<script lang="ts">
	let { date, timezone = undefined } = $props<{
		date: Date;
		timezone?: string;
	}>();

	const timezoneName = $derived(
		timezone ??
			new Intl.DateTimeFormat('en-US', { timeZoneName: 'shortOffset' })
				.formatToParts(date)
				.find((part) => part.type === 'timeZoneName')?.value
	);

	const formattedDate = $derived(
		new Intl.DateTimeFormat('en-US', {
			month: 'long',
			day: 'numeric',
			year: 'numeric',
			timeZone: timezone
		}).format(date)
	);

	const formattedTime = $derived(
		new Intl.DateTimeFormat('en-US', {
			hour: '2-digit',
			minute: '2-digit',
			second: '2-digit',
			hour12: true,
			timeZone: timezone
		}).format(date)
	);
</script>

<div aria-label="Time in {timezoneName}: {formattedDate} {formattedTime}" class="timezone-card">
	<span class="timezone-badge">
		{timezoneName}
	</span>
	<time dateTime={date.toISOString()}>{formattedDate}</time>
	<time class="time" dateTime={date.toISOString()}>
		{formattedTime}
	</time>
</div>

<style>
	.timezone-card {
		display: grid;
		grid-template-columns: auto 1fr auto;
		align-items: center;
		gap: 0.5rem;
		color: var(--muted-foreground);
		font-size: 0.875rem;
	}

	.timezone-badge {
		width: fit-content;
		border-radius: var(--radius);
		background-color: var(--accent);
		padding: 0.125rem 0.25rem;
		font-weight: 500;
		font-size: 0.75rem;
	}

	.time {
		font-variant-numeric: tabular-nums;
	}
</style>
