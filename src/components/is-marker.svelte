<script lang="ts">
	import type { RenderableMarker } from '../routes/types';

	export let marker: RenderableMarker;
</script>

{#if marker.markerType === 'birthday'}
	<span class="birthday">({marker.name})</span>
{/if}

{#if marker.markerType === 'today'}
	<span class="today" id="#today" />
{/if}

<style>
	span {
		display: inline;
		color: var(--color-life);
		font-family: monospace;
		letter-spacing: 0.5ch;
		user-select: none;
		word-break: break-all;
	}

	.birthday {
		letter-spacing: normal;
		position: relative;
		top: -0.07em;
	}

	.today {
		display: inline-block;
		line-height: 2;
		position: relative;
		top: 0.17em;
		left: -0.24rem;
		color: var(--color-link);
		position: relative;
		width: 1.2ch;
		height: 1em;
		padding: 0;
		letter-spacing: normal;
	}

	.today::after {
		content: '♥';
		position: absolute;
		left: 50%;
		top: 50%;
		translate: -50% -50%;
		display: block;
		font-size: 4em;
		animation: animateHeart 1s infinite;
	}

	@keyframes animateHeart {
		0% {
			scale: calc(3 * 0.8 / 9);
		}
		5% {
			scale: calc(3 * 0.9 / 9);
		}
		10% {
			scale: calc(3 * 0.8 / 9);
		}
		15% {
			scale: calc(3 * 1 / 9);
		}
		50% {
			scale: calc(3 * 0.8 / 9);
		}
		100% {
			scale: calc(3 * 0.8 / 9);
		}
	}
</style>
