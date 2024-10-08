<script lang="ts">
	import type { Content } from '@prismicio/client';
	import background from './background.jpg';
	import IconNpm from '~icons/fa-brands/npm';
	import IconGithub from '~icons/fa-brands/github';
	import IconFigma from '~icons/fa-brands/figma';
	import IconFly from '~icons/fa-brands/fly';
	import IconCloudflare from '~icons/fa-brands/cloudflare';
	import IconDigitalOcean from '~icons/fa-brands/digital-ocean';

	import Bounded from '$lib/components/Bounded.svelte';
	import LogoBackground from './LogoBackground.svelte';
	import StylizedLogoMark from './StylizedLogoMark.svelte';
	import { PrismicText, PrismicRichText } from '@prismicio/svelte';
	import clsx from 'clsx';
	import { onMount } from 'svelte';
	import gsap from 'gsap';
	export let slice: Content.IntegrationsSlice;

	const icons = {
		npm: IconNpm,
		github: IconGithub,
		figma: IconFigma,
		fly: IconFly,
		cloudflare: IconCloudflare,
		digitalocean: IconDigitalOcean
	};
	/** @type {import("@prismicio/client").Content.IntegrationsSlice}*/
	onMount(() => {
		const tl = gsap.timeline({
			repeat: -1,
			defaults: { ease: 'power2.inOut' }
		});

		tl.to('.pulsing-logo', {
			keyframes: [
				{
					filter: 'brightness(2)',
					opacity: 1,
					duration: 0.4,
					ease: 'power2.in'
				},
				{ filter: 'brightness(1)', opacity: 0.7, duration: 0.9 }
			]
		});

		tl.to(
			'.signal-line',
			{
				keyframes: [
					{
						backgroundPosition: '0% 0%'
					},
					{
						backgroundPosition: '100% 100%',
						stagger: { from: 'center', each: 0.3 },
						duration: 1
					}
				]
			},
			'-=1.4'
		);

		tl.to(
			'.pulsing-icon',
			{
				keyframes: [
					{
						opacity: 1,
						stagger: {
							from: 'center',
							each: 0.3
						},
						duration: 1
					},
					{
						opacity: 0.4,
						stagger: {
							from: 'center',
							each: 0.3
						},
						duration: 1
					}
				]
			},
			'-=2'
		);
	});
</script>

<Bounded
	data-slice-type={slice.slice_type}
	data-slice-variation={slice.variation}
	class="relative overflow-hidden"
>
	<img src={background} alt="" class="absolute inset-0 h-full w-full object-cover" />
	<LogoBackground />
	<div class="relative">
		<h2
			class="mx-auto max-w-2xl text-balance bg-gradient-to-b from-violet-50 to-violet-300
	bg-clip-text py-2 text-center text-5xl font-medium text-transparent md:text-7xl"
		>
			<PrismicText field={slice.primary.heading} />
		</h2>
		<div class="mx-auto mt-6 max-w-md text-balance text-center text-gray-300">
			<PrismicRichText field={slice.primary.body} />
		</div>

		<div class="mt-20 flex flex-col items-center md:flex-row">
			{#each slice.primary.repeat as item, index}
				{#if index === Math.floor(slice.primary.repeat.length / 2)}
					<StylizedLogoMark />
					<div class="signal-line rotate-180"></div>
				{/if}
				{#if item.icon}
					<div
						class="pulsing-icon text-violet flex aspect-square shrink-0 items-center justify-center
			rounded-full border border-violet-50/30 bg-violet-50/25 p-3 text-3xl opacity-40 md:text-3xl
			lg:text-5xl"
					>
						<svelte:component this={icons[item.icon]} />
					</div>
				{/if}
				{#if index !== slice.primary.repeat.length - 1}
					<div
						class={clsx(
							'signal-line',
							index >= Math.floor(slice.primary.repeat.length / 2) ? 'rotate-180' : 'rotate-0'
						)}
					></div>
				{/if}
			{/each}
		</div>
	</div>
</Bounded>
