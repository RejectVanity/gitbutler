<script lang="ts">
	import FullscreenLoading from '$lib/components/FullscreenLoading.svelte';
	import PullRequestPreview from '$lib/components/PullRequestPreview.svelte';
	import { GitHubService } from '$lib/github/service';
	import { getContextByClass } from '$lib/utils/context';
	import { map } from 'rxjs';
	import { page } from '$app/stores';

	const githubService = getContextByClass(GitHubService);

	$: pr$ = githubService.prs$?.pipe(
		map((prs) => prs.find((b) => b.number.toString() == $page.params.number))
	);
</script>

<div class="wrapper overflow-y-auto overscroll-none">
	<div class="inner flex">
		{#if !$pr$}
			<FullscreenLoading />
		{:else if pr$}
			<PullRequestPreview pullrequest={$pr$} />
		{:else}
			<p>Branch doesn't seem to exist</p>
		{/if}
	</div>
</div>

<style lang="postcss">
	.wrapper {
		display: flex;
		height: 100%;
		overflow-y: auto;
	}
	.inner {
		padding: var(--size-16);
	}
</style>
