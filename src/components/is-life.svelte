<script lang="ts">
	import type { RenderableGap } from '../routes/types';

	export let day: RenderableGap;

	// Instead of a single string of dots, create an array of dates
	$: dates = Array(day.duration).fill(0).map((_, index) => {
		const date = new Date(day.start);
		date.setDate(date.getDate() + index);
		return date;
	});

	$: isFuture = new Date(day.start).getTime() > new Date().getTime();

	const formatDate = (date: Date) => {
		return date.toLocaleDateString('en-US', {
			year: 'numeric',
			month: 'long',
			day: 'numeric'
		});
	}

	// Track the hovered date and position
	let hoveredDate: Date | null = null;
	let tooltipX = 0;
	let tooltipY = 0;
	
	// Handle mouse events
	function handleMouseEnter(date: Date, event: MouseEvent) {
		hoveredDate = date;
		updateTooltipPosition(event);
	}
	
	function handleMouseLeave() {
		hoveredDate = null;
	}
	
	function updateTooltipPosition(event: MouseEvent) {
		tooltipX = event.clientX + 10; // Offset from cursor
		tooltipY = event.clientY - 30; // Position above cursor
	}
	
	function handleMouseMove(event: MouseEvent) {
		if (hoveredDate) {
			updateTooltipPosition(event);
		}
	}
</script>

<span class:is-future={isFuture}>
	{#each dates as date, i}
		<span 
			class="dot" 
			role="button"
			aria-label={formatDate(date)}
			on:mouseenter={(e) => handleMouseEnter(date, e)}
			on:mouseleave={handleMouseLeave}
			on:mousemove={handleMouseMove}
		>·</span>{#if i < dates.length - 1}<span class="spacer"></span>{/if}
	{/each}
</span>

{#if hoveredDate}
	<div class="tooltip" style="left: {tooltipX}px; top: {tooltipY}px;">
		{formatDate(hoveredDate)}
	</div>
{/if}

<style>
	span {
		display: inline;
		color: var(--color-life);
		font-family: monospace;
		user-select: none;
		-webkit-user-select: none;
		cursor: default;
		word-break: break-all;
	}

	.dot {
		cursor: help;
		display: inline-block;
	}

	.spacer {
		display: inline-block;
		width: 0.5ch;
	}

	span:first-child {
		padding-inline-start: 0;
	}

	.is-future {
		color: var(--color-future);
	}
	
	.tooltip {
		position: fixed;
		z-index: 100;
		background-color: var(--color-bg);
		color: var(--color-text);
		padding: 4px 8px;
		border-radius: 4px;
		font-size: 0.8rem;
		box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
		pointer-events: none;
		white-space: nowrap;
		border: 1px solid var(--color-life);
		animation: fadeIn 0.1s;
	}
	
	@keyframes fadeIn {
		from { opacity: 0; }
		to { opacity: 1; }
	}
</style>
