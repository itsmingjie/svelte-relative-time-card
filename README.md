# Svelte Relative Time Card

A hover card component that displays relative time relative to local time with timezone information. Built for Svelte 5.

## Installation

```bash
npm install svelte-relative-time-card
# or
pnpm add svelte-relative-time-card
# or
yarn add svelte-relative-time-card
```

## Usage

Import the component and its styles:

```svelte
<script lang="ts">
	import { RelativeTimeCard } from 'svelte-relative-time-card';
</script>

<RelativeTimeCard date={new Date()} />
```

## API

### Props

| Prop             | Type                                     | Default     | Description                                                          |
| ---------------- | ---------------------------------------- | ----------- | -------------------------------------------------------------------- |
| `date`           | `Date \| string \| number`               | Required    | The date to display. Can be a Date object, ISO string, or timestamp. |
| `timezones`      | `string[]`                               | `["UTC"]`   | List of timezones to display in the hover card.                      |
| `updateInterval` | `number`                                 | `1000`      | Interval in milliseconds to update the relative time display.        |
| `variant`        | `"default" \| "muted" \| "ghost"`        | `"default"` | The visual style of the trigger element.                             |
| `side`           | `"top" \| "right" \| "bottom" \| "left"` | `"top"`     | The preferred side of the trigger to render against when open.       |
| `align`          | `"start" \| "center" \| "end"`           | `"center"`  | The preferred alignment against the trigger.                         |
| `sideOffset`     | `number`                                 | `4`         | The distance in pixels from the trigger.                             |
| `alignOffset`    | `number`                                 | `0`         | An offset in pixels from the "start" or "end" alignment options.     |
| `openDelay`      | `number`                                 | `500`       | The delay in milliseconds before the hover card opens.               |
| `closeDelay`     | `number`                                 | `300`       | The delay in milliseconds before the hover card closes.              |
| `trigger`        | `(props: { dateObj: Date }) => Snippet`  | -           | Custom trigger element render function.                              |

### Variants

The component supports three visual variants for the trigger element:

- `default`: Standard appearance with 70% opacity, increasing to 90% on hover
- `muted`: Subtle appearance with 50% opacity, increasing to 70% on hover
- `ghost`: Text-only appearance with underline on hover

## Examples

### Basic Usage

```svelte
<RelativeTimeCard date={new Date()} />
```

### Multiple Timezones

```svelte
<RelativeTimeCard
	date={new Date()}
	timezones={['America/New_York', 'Europe/London', 'Asia/Tokyo']}
/>
```

### Custom Trigger

```svelte
<RelativeTimeCard date={new Date()}>
	{#snippet trigger({ dateObj })}
		<button class="custom-trigger">
			<Clock class="icon" size={16} />
			View time details
		</button>
	{/snippet}
</RelativeTimeCard>
```

### Different Positions

```svelte
<RelativeTimeCard date={new Date()} side="top" align="start">
	{#snippet trigger({ dateObj })}
		Top Start
	{/snippet}
</RelativeTimeCard>

<RelativeTimeCard date={new Date()} side="right" align="center">
	{#snippet trigger({ dateObj })}
		Right Center
	{/snippet}
</RelativeTimeCard>

<RelativeTimeCard date={new Date()} side="bottom" align="end">
	{#snippet trigger({ dateObj })}
		Bottom End
	{/snippet}
</RelativeTimeCard>
```

## Accessibility

The component is fully keyboard accessible:

- Focusable via Tab key
- Shows preview on focus
- Hides preview on blur
- Supports custom trigger elements

## Styling

The component uses CSS variables for theming. You can customize the appearance by setting these variables in your CSS:

```css
:root {
	--background: hsl(0 0% 100%);
	--foreground: hsl(240 10% 3.9%);
	--muted: hsl(240 4.8% 95.9%);
	--muted-foreground: hsl(240 3.8% 46.1%);
	--popover: hsl(0 0% 100%);
	--popover-foreground: hsl(240 10% 3.9%);
	--border: hsl(240 5.9% 90%);
	--ring: hsl(240 5% 64.9%);
	--radius: 0.5rem;
}
```

## Credits

- [Dice UI](https://www.diceui.com/docs/components/relative-time-card): React implementation by [@sadmann7](https://github.com/sadmann7)
- [@JohnPhamous](https://x.com/JohnPhamous/status/1836091528911216803) for original idea

## License

MIT
