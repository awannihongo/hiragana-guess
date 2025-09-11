<script lang="ts">
	import { Button } from '@/components/ui/button';
	import { Input } from '@/components/ui/input';
	import { filterColumn } from '@/mapping';
	import { shuffle } from '@/shuffle';
	import { toggleMode } from 'mode-watcher';
	import { toast } from 'svelte-sonner';

	let {
		sessionPage = $bindable(),
		daku,
		handa,
		yoon,
		answerMode
	}: {
		sessionPage: boolean;
		daku: boolean;
		handa: boolean;
		yoon: boolean;
		answerMode: 'BUTTONS' | 'TEXT_INPUT';
	} = $props();

	type Answer = {
		value: string;
		wrong: boolean;
	};

	const {
		hiragana2roman,
		keys: hiragana2romanKeys,
		values: hiragana2romanValues
	} = filterColumn(daku, handa, yoon);

	const getRandomSessionAnswers = () => {
		return shuffle(hiragana2romanKeys);
	};

	let sessionAnswers = $state(getRandomSessionAnswers());
	let sessionIndex = $state(0);
	let sessionProgress = $derived.by(() => {
		return `${sessionIndex} / ${sessionAnswers.length}`;
	});

	const getRandomHiraganaAnswers = () => {
		const character = sessionAnswers[sessionIndex];
		const answer = hiragana2roman.get(character)!;

		const answers = shuffle([
			answer,
			...shuffle(
				hiragana2romanValues.filter((v) => v != answer && v.length === answer.length)
			).slice(0, 3)
		]).map((v) => ({ value: v, wrong: false }) as Answer);

		return {
			character,
			answer,
			answers
		};
	};

	let hiragana = $state(getRandomHiraganaAnswers());

	const setAnswer = (answer: string, index: number) => {
		if (answer === hiragana.answer) {
			sessionIndex += 1;
			if (sessionIndex >= sessionAnswers.length) {
				sessionAnswers = getRandomSessionAnswers();
				sessionIndex = 0;
			}
			hiragana = getRandomHiraganaAnswers();
		} else {
			hiragana.answers[index].wrong = true;
		}
	};

	let submitAnswerValue = $state('');

	const submitAnswer = (answer: string) => {
		if (answer === hiragana.answer) {
			sessionIndex += 1;
			if (sessionIndex >= sessionAnswers.length) {
				sessionAnswers = getRandomSessionAnswers();
				sessionIndex = 0;
			}
			hiragana = getRandomHiraganaAnswers();
		} else {
			toast.error(`${answer} bukan jawaban yang benar`);
		}
		submitAnswerValue = '';
	};
</script>

<div class="column flex min-h-dvh flex-col">
	<div class="mx-auto flex w-full max-w-2xl flex-row items-center gap-4 p-4 md:gap-4">
		<Button
			onclick={() => {
				sessionPage = false;
			}}
			size="icon"
			variant="outline"
			class="text-2xl md:text-3xl"
		>
			<svg
				xmlns="http://www.w3.org/2000/svg"
				width="32"
				height="32"
				viewBox="0 0 24 24"
				fill="none"
				stroke="currentColor"
				stroke-width="3"
				stroke-linecap="round"
				stroke-linejoin="round"
				class="lucide lucide-chevron-left"
			>
				<path d="m15 18-6-6 6-6" />
			</svg>
		</Button>
		<Button onclick={toggleMode} size="icon" variant="outline" class="text-2xl md:text-3xl">
			<svg
				xmlns="http://www.w3.org/2000/svg"
				width="32"
				height="32"
				viewBox="0 0 24 24"
				fill="none"
				stroke="currentColor"
				stroke-width="3"
				stroke-linecap="round"
				stroke-linejoin="round"
				class="lucide lucide-lightbulb"
			>
				<path
					d="M15 14c.2-1 .7-1.7 1.5-2.5 1-.9 1.5-2.2 1.5-3.5A6 6 0 0 0 6 8c0 1 .2 2.2 1.5 3.5.7.7 1.3 1.5 1.5 2.5"
				/>
				<path d="M9 18h6" />
				<path d="M10 22h4" />
			</svg>
		</Button>
		<span class="ml-auto font-semibold tabular-nums">
			{sessionProgress}
		</span>
	</div>

	<div class="flex flex-1 items-center justify-center font-jp text-[10rem] md:text-[12rem]">
		{hiragana.character}
	</div>

	{#if answerMode === 'BUTTONS'}
		<div class="mx-auto flex w-full max-w-2xl flex-row justify-stretch gap-4 p-4 md:gap-4">
			{#each hiragana.answers as answer, i}
				<Button
					onclick={() => {
						setAnswer(answer.value, i);
					}}
					variant={answer.wrong ? 'destructive' : 'default'}
					disabled={answer.wrong}
					size="lg"
					class="grow px-0 py-8 text-2xl md:text-3xl"
				>
					{answer.value}
				</Button>
			{/each}
		</div>
	{/if}

	{#if answerMode === 'TEXT_INPUT'}
		<form
			onsubmit={(event) => {
				event.preventDefault();
				submitAnswer(submitAnswerValue.toLowerCase());
			}}
			class="mx-auto flex w-full max-w-2xl flex-row items-center justify-stretch gap-4 p-4 md:gap-4"
		>
			<Input
				type="answer"
				placeholder="Jawaban"
				bind:value={submitAnswerValue}
				class="py-8 text-2xl md:text-3xl"
			/>
			<Button
				type="submit"
				size="lg"
				class="py-8 text-2xl uppercase md:text-3xl"
				disabled={submitAnswerValue === ''}>submit</Button
			>
		</form>
	{/if}
</div>
