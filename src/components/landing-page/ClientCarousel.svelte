<script lang="ts">
	import Nasa from "../../assets/branding/landing_imgs/clients/nasa2.png";
	import TicketMaster from "../../assets/branding/landing_imgs/clients/ticketmaster.png";
	import Ubs from "../../assets/branding/landing_imgs/clients/ubs.png";
	import Leonardo from "../../assets/branding/landing_imgs/clients/leonardo.png";
	import Blackbird from "../../assets/branding/landing_imgs/clients/blackbird.png";
	import Phet from "../../assets/branding/landing_imgs/clients/phet.png";
	import Siemens from "../../assets/branding/landing_imgs/clients/siemens.png";

	const clientImages = [
		{ src: Nasa, alt: "NASA" },
		{ src: TicketMaster, alt: "TicketMaster" },
		{ src: Ubs, alt: "UBS" },
		{ src: Leonardo, alt: "Leonardo" },
		{ src: Blackbird, alt: "Blackbird" },
		{ src: Phet, alt: "Phet" },
		{ src: Siemens, alt: "Siemens" },
	];

	// To do the scroll animation, we need to repeat the images
	const clientImages2x = [...clientImages, ...clientImages, clientImages[0]!];

	let loadedImagesCount = 0;

	const imgEls: HTMLImageElement[] = [];
	$: {
		// on:load won't fire if image is already loaded (complete)
		for (const el of imgEls) {
			if (el.complete) {
				loadedImagesCount++;
			}
		}
	}

	// Once all images are loaded, detect where the first repetition starts and begin animation
	let repeatWidth: number | undefined;
	$: {
		if (loadedImagesCount === clientImages2x.length) {
			const repetitionStartImg = imgEls[clientImages.length];
			repeatWidth = repetitionStartImg!.parentElement!.offsetLeft;
		}
	}
</script>

<div class="w-full relative mx-auto 2xl:max-w-screen-2xl opacity-0 transition-opacity" class:opacity-100={typeof repeatWidth === "number"}>
	<ul
		class="client-carousel whitespace-nowrap mx-auto 2xl:!animate-none 2xl:overflow-x-hidden"
		aria-label="Clients"
		style:--repeat-width={repeatWidth + "px"}
		style:animation-name={typeof repeatWidth === "undefined" ? "unset" : "client-carousel"}
	>
		{#each clientImages2x as { src, alt }, i}
			<li
				class="inline-block px-8"
			>
				<img
					src={src.src}
					alt={alt}
					decoding="async"
					class="opacity-50 h-16 grayscale contrast-0"
					bind:this={imgEls[i]}
					on:load={() => loadedImagesCount++}
				/>
			</li>
		{/each}
	</ul>
</div>

<style>
	.client-carousel {
		animation: client-carousel 10s linear infinite;
		width: var(--repeat-width);
	}

	@keyframes -global-client-carousel {
		0% {
			transform: translateX(0%);
		}
		100% {
			transform: translateX(calc(var(--repeat-width) * -1));
		}
	}
</style>
