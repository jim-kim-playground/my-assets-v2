<script lang="ts">
	import Chart from 'chart.js/auto/auto.esm';
	import { Interaction } from 'chart.js';
	import { CrosshairPlugin, Interpolate } from 'chartjs-plugin-crosshair';
	import 'chartjs-adapter-date-fns';
	import type { ChartType, TimeUnit } from 'chart.js/auto';
	import { onMount, onDestroy } from 'svelte';

	export let lineData = {
		meta: {
			symbol: '',
			dataGranularity: '',
		},
		dataset: []
	};

	let lineChart = null;

	//TODO 'config' type check error because of chart.js defect I GUESS
	onMount(() => {
		const config = generateConfig(lineData);
		Chart.register(CrosshairPlugin);
		Interaction.modes.interpolate = Interpolate;
		lineChart = new Chart(
			document.getElementById(`line-chart-${lineData.meta.symbol}`) as HTMLCanvasElement,
			config
		);
	});

	onDestroy(() => {
		if (lineChart) {
			//TODO Why onDestroy is called when component is initialized in server
			lineChart.destroy();
		}
	});

	const generateConfig = (data) => {
		const config = {
			type: 'line' as ChartType,
			data: {
				datasets: [
					{
						label: data.symbol,
						backgroundColor: 'rgba(37, 99, 235, 0.25)',
						borderColor: 'rgb(37, 99, 235)',
						pointBackgroundColor: 'rgb(102, 126, 234)',
						fill: true,
						cubicInterpolationMode: 'monotone',
						data: data.datasets
					}
				]
			},
			options: {
				elements: {
					point: {
						pointStyle: 'point'
					}
				},
				scales: {
					x: {
						type: 'time',
						time: {
							unit: 'day' as TimeUnit,
							displayFormats: {
								datetime: 'MMM d, yyyy, h:mm:ss a',
								millisecond: 'h:mm:ss.SSS a',
								second: 'hh:mm:ss',
								minute: 'hh:mm',
								hour: 'ha',
								day: 'MM/dd',
								week: 'w',
								month: 'MMM yyyy',
								quarter: '[Q]Q - yyyy',
								year: 'yyyy'
							}
						}
					}
				},
				plugins: {
					legend: {
						display: false
					},
					tooltip: {
						mode: 'interpolate',
						intersect: false
					},
					crosshair: {
						line: {
							color: '#F66',
							width: 1
						},
						sync: {
							enabled: true,
							group: 1,
							suppressTooltips: true
						},
						zoom: {
							enabled: false
						}
					}
				}
			}
		};

		return config;
	};
</script>

<div class="w-full">
	<canvas id="line-chart-{lineData.meta.symbol}" class="bg-slate-100 dark:bg-slate-800" />
</div>
