<script lang="ts">
	import type { Detailed, Report } from 'lib/model/Report';
	import type { Query } from 'lib/reports/ReportQuery';
	import millisecondsToTimeString from 'lib/util/millisecondsToTimeString';
	import moment from 'moment';
	import TimeEntryDetailed from '../components/reports/lists/TimeEntryDetailed.svelte';
	import type { ReportDetailedItem } from '../components/reports/lists/types';
	import JsError from './JSError.svelte';

	export let query: Query;
	export let detailed: Report<Detailed>;

	let _error: string;

	let _listData: ReportDetailedItem[];

	$: if (query && detailed) {
		try {
			_error = null;
			_listData = getListData(detailed, query);
		} catch (err) {
			_error = err.stack;
			console.error(err);
		}
	}

	function getListData(
		report: Report<Detailed>,
		query: Query
	): ReportDetailedItem[] {
		return report.data
			.map(
				(r: Detailed): ReportDetailedItem => ({
					name: r.description,
					totalTime: millisecondsToTimeString(r.dur),
					hex: r.project_hex_color,
					order: moment(r.start).unix(),
					tags: r.tags,
					project: r.project,
					client: r.client,
					startEnd: `${moment(r.start).format('HH:mm')} - ${moment(
						r.end
					).format('HH:mm')}`
					// startEnd: r.start + '-' + r.end
				})
			)
			.reverse();
	}
</script>

{#if _error}
	<JsError message={_error} />
{:else}
	<TimeEntryDetailed data={_listData} />
{/if}
