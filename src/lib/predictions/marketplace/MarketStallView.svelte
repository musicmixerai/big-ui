<script lang="ts">
	import { goto } from '$app/navigation';
	import MarkdownRenderer from '$lib/components/ui/MarkdownRenderer.svelte';
	import { type MarketData, type PredictionMarketCreateEvent, type Sip10Data } from '@mijoco/stx_helpers/dist/index';
	import { onMount } from 'svelte';
	import { getMarketToken, totalPoolSum } from '../predictions';
	import ExchangeRate from '$lib/components/common/ExchangeRate.svelte';
	import { fmtMicroToStx } from '$lib/utils';
	import YesNoButtons from './YesNoButtons.svelte';
	import LogoContainer from './LogoContainer.svelte';
	import { stakeAmount, stakeAmountHome } from '$stores/stores';
	import ExchangeRateHome from '$lib/components/common/ExchangeRateHome.svelte';

	export let market: PredictionMarketCreateEvent;
	let totalPoolMicro = 0;
	let inited = false;
	let sip10Data: Sip10Data;

	onMount(async () => {
		sip10Data = getMarketToken(market.marketData.token);
		totalPoolMicro = totalPoolSum(market.marketData.stakes);
		stakeAmountHome.set(totalPoolMicro);
		inited = true;
	});
</script>

<!-- {#if showModal}
	<SlotModal onClose={() => closeModal()}>
		<div class="" slot="modalBody"><PollVote {market} onSubmitPollVote={handleSubmitPollVote} /></div>
	</SlotModal>
{/if} -->

{#if market}
	<!-- Market Logo -->
	<div class="mb-10 flex items-start gap-x-5 text-black">
		<LogoContainer logo={market.unhashedData.logo} />
		<h2 class="font-inter text-[30px] font-bold leading-[1.1] tracking-tight md:text-[40px]">
			<a href={`/market/${market.marketId}/${market.marketType}`} class="hover:text-blue-1000">{market.unhashedData.name}</a>
		</h2>
	</div>

	<!-- Market Actions -->
	<YesNoButtons {market} />
	<div class="my-10">
		<div class="font-inter font-medium text-black"><MarkdownRenderer value={market.unhashedData.description} /></div>
		<div class="font-inter font-medium text-black"><MarkdownRenderer value={market.unhashedData.criteria} /></div>
	</div>
	<div class="mt-5">
		<div class="mb-5 font-inter font-bold text-black">
			{#if sip10Data}
				<!-- TVL: {fmtMicroToStx(totalPoolSum(marketData?.stakes))} -->
				TVL: <ExchangeRateHome {sip10Data} />
			{/if}
		</div>
	</div>
{/if}
