<script lang="ts">
	import { Button } from '@/components/ui/button';
	import { Checkbox } from '@/components/ui/checkbox';
	import { Input } from '@/components/ui/input';
	import { Label } from '@/components/ui/label';
	import * as RadioGroup from '@/components/ui/radio-group/index';

	let {
		sessionPage = $bindable(),
		daku = $bindable(),
		handa = $bindable(),
		yoon = $bindable(),
		answerMode = $bindable()
	}: {
		sessionPage: boolean;
		daku: boolean;
		handa: boolean;
		yoon: boolean;
		answerMode: 'BUTTONS' | 'TEXT_INPUT';
	} = $props();
</script>

<div class="column flex min-h-dvh flex-col">
	<div class="mx-auto mt-auto flex flex w-full max-w-2xl flex-1 flex-col justify-center gap-2 p-4">
		<h3 class="scroll-m-20 text-xl font-semibold uppercase tracking-tight">Variasi</h3>
		<div class="flex items-center space-x-2">
			<Checkbox id="dakuten" bind:checked={daku} aria-labelledby="dakuten-label" />
			<Label
				id="dakuten-label"
				for="dakuten"
				class="text-md font-medium leading-none peer-disabled:cursor-not-allowed peer-disabled:opacity-70"
			>
				Dakuten
				<a href="//en.wikipedia.org/wiki/Dakuten_and_handakuten" target="_blank" aria-label="Help">
					<svg
						xmlns="http://www.w3.org/2000/svg"
						width="18"
						height="18"
						viewBox="0 0 24 24"
						fill="none"
						stroke="currentColor"
						stroke-width="2"
						stroke-linecap="round"
						stroke-linejoin="round"
						class="inline"
					>
						<circle cx="12" cy="12" r="10" />
						<path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3" />
						<path d="M12 17h.01" />
					</svg>
				</a>
			</Label>
		</div>

		<div class="flex items-center space-x-2">
			<Checkbox id="handakuten" bind:checked={handa} aria-labelledby="handakuten-label" />
			<Label
				id="handakuten-label"
				for="handakuten"
				class="text-md font-medium leading-none peer-disabled:cursor-not-allowed peer-disabled:opacity-70"
			>
				Handakuten
				<a href="//en.wikipedia.org/wiki/Dakuten_and_handakuten" target="_blank" aria-label="Help">
					<svg
						xmlns="http://www.w3.org/2000/svg"
						width="18"
						height="18"
						viewBox="0 0 24 24"
						fill="none"
						stroke="currentColor"
						stroke-width="2"
						stroke-linecap="round"
						stroke-linejoin="round"
						class="inline"
					>
						<circle cx="12" cy="12" r="10" />
						<path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3" />
						<path d="M12 17h.01" />
					</svg>
				</a>
			</Label>
		</div>

		<div class="flex items-center space-x-2">
			<Checkbox id="yoon" bind:checked={yoon} aria-labelledby="yoon-label" />
			<Label
				id="yoon-label"
				for="yoon"
				class="text-md font-medium leading-none peer-disabled:cursor-not-allowed peer-disabled:opacity-70"
			>
				Yōon
				<a href="//en.wikipedia.org/wiki/Y%C5%8Don" target="_blank" aria-label="Help">
					<svg
						xmlns="http://www.w3.org/2000/svg"
						width="18"
						height="18"
						viewBox="0 0 24 24"
						fill="none"
						stroke="currentColor"
						stroke-width="2"
						stroke-linecap="round"
						stroke-linejoin="round"
						class="inline"
					>
						<circle cx="12" cy="12" r="10" />
						<path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3" />
						<path d="M12 17h.01" />
					</svg>
				</a>
			</Label>
		</div>
		<h3 class="mt-2 scroll-m-20 text-xl font-semibold uppercase tracking-tight">Mode Jawab</h3>
		<RadioGroup.Root bind:value={answerMode} class="grid grid-cols-2">
			<div class="flex flex-col gap-4">
				<div class="flex items-center space-x-2">
					<RadioGroup.Item value="BUTTONS" id="r1" />
					<Label for="r1">Tombol</Label>
				</div>
				<div class="flex aspect-[1/1] flex-col justify-end rounded border-4 p-2 sm:aspect-[2/1]">
					<div class="flex flex-row gap-2">
						<Button class="grow px-0"></Button>
						<Button class="grow px-0"></Button>
						<Button class="grow px-0" disabled variant="destructive"></Button>
						<Button class="grow px-0"></Button>
					</div>
				</div>
			</div>
			<div class="flex flex-col gap-4">
				<div class="flex items-center space-x-2">
					<RadioGroup.Item value="TEXT_INPUT" id="r2" />
					<Label for="r2">Teks</Label>
				</div>
				<div class="flex aspect-[1/1] flex-col justify-end rounded border-4 p-2 sm:aspect-[2/1]">
					<div class="flex flex-row gap-2">
						<div class="flex w-full max-w-sm items-center space-x-2">
							<Input type="answer" placeholder="Jawaban" />
							<Button>SUBMIT</Button>
						</div>
					</div>
				</div>
			</div>
		</RadioGroup.Root>
	</div>

	<div class="mx-auto mt-auto flex w-full max-w-2xl flex-row justify-stretch gap-4 p-4 md:gap-4">
		<Button
			onclick={() => {
				sessionPage = true;
			}}
			variant="default"
			size="lg"
			class="grow px-0 py-8 text-2xl md:text-3xl">MULAI</Button
		>
	</div>
</div>
