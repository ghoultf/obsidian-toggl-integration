<script lang="ts">
	import type { ReportDetailedItem } from './types';
	import TimerTag from '../../current_timer/TimerTag.svelte';

	export let data: ReportDetailedItem[];
</script>

<div class="time-entry-list">
	<div
		class="time-entry-group-header is-flex is-justify-content-space-between pb-1 mb-2"
	>
		<div class="is-flex is-align-items-center">
			<span>Time Entry</span>
		</div>
		<div class="is-flex">
			<div class="header-right">Duration</div>
			<div class="header-right">Time</div>
		</div>
	</div>
	<div class="group-items">
		{#each data as d}
			<div
				class="group-item mb-2 is-flex is-justify-content-space-between"
			>
				<div class="is-flex is-align-items-center">
					<span class="mr-2">{d.name}</span>
					<div
						aria-label={d.project || 'No project'}
						class="project-circle mr-2"
						style="background-color:{d.hex || 'var(--text-muted)'}"
					/>
					<span class="mr-2">
						<span>{d.project ? d.project : '(No project)'}</span>
						{#if d.project}
							<span class="project-client">• {d.client}</span>
						{/if}
					</span>
					{#each d.tags as tag}
						<span class="mr-1"><TimerTag name={tag} /></span>
					{/each}
				</div>

				<div class="is-flex">
					<div class="group-item-time">{d.totalTime}</div>
					<div class="group-item-time">{d.startEnd}</div>
				</div>
			</div>
		{/each}
	</div>
</div>

<style>
	.project-client {
		color: var(--text-muted);
		font-weight: 300;
		display: inline-block;
	}

	.group-item-time {
		width: 7rem;
	}

	.header-right {
		width: 7rem;
		text-align: right;
	}

	@media (max-width: 640px) {
	}
</style>
