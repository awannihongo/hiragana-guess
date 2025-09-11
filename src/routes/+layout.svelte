<script lang="ts">
	import '@fontsource/inter/400.css';
	import '@fontsource/inter/500.css';
	import '@fontsource/inter/600.css';
	import '@fontsource/inter/700.css';
	import '@fontsource/jetbrains-mono/400.css';
	import '@fontsource/noto-sans-jp/400.css';
	import '../app.css';
	import { ModeWatcher } from 'mode-watcher';
	import { Toaster } from '@/components/ui/sonner';

	let { children } = $props();
</script>

<Toaster position="top-center" />
<ModeWatcher />
<div class="min-h-dvh font-main">
	{@render children()}
</div>
