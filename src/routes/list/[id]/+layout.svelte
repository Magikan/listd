<script lang="ts">
	import { page } from '$app/stores';
	import YouTubeVideoEmbed from '$/lib/YouTubeVideoEmbed.svelte';
	import ChannelCard from '$/routes/(protected)/create/ChannelCard.svelte';
	import { ProgressRadial } from '@skeletonlabs/skeleton';

	export let data;
</script>

<slot />
{#if !$page.params.videoid}
	<h2 class="font-bold">{data.list?.title}</h2>
	<p>{data.list?.description}</p>
	<div
		class="my-4 grid grid-cols-1 place-content-center md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4">
		{#each data.list.items as item}
			{#if item.meta.youtubeMeta}
				<ChannelCard locale={data.locale} channel={item.meta.youtubeMeta} />
			{/if}
		{/each}
	</div>
{/if}
<div>
	{#await data.streamed.videos}
		<span class="grid w-full place-items-center p-4">
			<ProgressRadial class="ml-2 h-8 w-8" stroke={100} />
		</span>
	{:then videos}
		<div
			class="grid grid-cols-1 gap-4 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 2xl:grid-cols-5">
			{#each videos as video}
				<YouTubeVideoEmbed
					active={$page.params.videoid === video.videoId}
					locale={data.locale}
					{video} />
			{/each}
		</div>
	{/await}
</div>
