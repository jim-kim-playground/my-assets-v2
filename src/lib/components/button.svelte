<script lang="ts">
	import { ComponentSize, ComponentColors, ButtonType } from '$lib/components/enums';

	export let type: ButtonType = ButtonType.Text;
	export let size: ComponentSize = ComponentSize.Medium;
	export let colors: ComponentColors = ComponentColors.Primary;
	export let isDisplay = true;
</script>

<button class="{type} {size} {colors} {isDisplay ? 'block' : 'hidden'} rounded mx-2 px-4" on:click>
	<slot />
</button>

<style lang="postcss">
	.primary {
		@apply bg-blue-700 text-slate-300 hover:bg-blue-900 ripple-bg-blue-900;
	}
	:global(.dark) .primary {
		@apply bg-blue-600 text-slate-100 hover:bg-blue-800 ripple-bg-blue-800;
	}
	.lg {
		@apply h-9;
	}
	.md {
		@apply h-8;
	}
	.sm {
		@apply h-7;
	}
</style>
