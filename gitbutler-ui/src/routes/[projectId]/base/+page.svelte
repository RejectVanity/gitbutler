<script lang="ts">
	import BaseBranch from '$lib/components/BaseBranch.svelte';
	import FileCard from '$lib/components/FileCard.svelte';
	import FullscreenLoading from '$lib/components/FullscreenLoading.svelte';
	import Resizer from '$lib/components/Resizer.svelte';
	import ScrollableContainer from '$lib/components/ScrollableContainer.svelte';
	import { SETTINGS_CONTEXT, type SettingsStore } from '$lib/settings/userSettings';
	import { getContextByClass } from '$lib/utils/context';
	import { BaseBranchService } from '$lib/vbranches/branchStoresCache';
	import { Ownership } from '$lib/vbranches/ownership';
	import lscache from 'lscache';
	import { getContext, onMount } from 'svelte';
	import { writable } from 'svelte/store';
	import type { AnyFile } from '$lib/vbranches/types';
	import type { PageData } from './$types';

	export let data: PageData;

	const defaultBranchWidthRem = 30;
	const laneWidthKey = 'historyLaneWidth';
	const selectedFiles = writable<AnyFile[]>([]);
	const userSettings = getContext<SettingsStore>(SETTINGS_CONTEXT);

	const baseBranchService = getContextByClass(BaseBranchService);
	const baseBranch = baseBranchService.base;

	let rsViewport: HTMLDivElement;
	let laneWidth: number;

	$: project$ = data.project$;
	$: projectId = data.projectId;
	$: error$ = baseBranchService.error$;

	$: projectPath = $project$.path;

	$: selectedOwnership = writable(Ownership.default());
	$: selected = setSelected($selectedFiles);

	function setSelected(files: AnyFile[]) {
		if (files.length == 0) return undefined;
		return files[0];
	}

	onMount(() => {
		laneWidth = lscache.get(laneWidthKey);
	});
</script>

{#if $error$}
	<p>Error...</p>
{:else if !$baseBranch}
	<FullscreenLoading />
{:else}
	<div class="base">
		<div
			class="base__left"
			bind:this={rsViewport}
			style:width={`${laneWidth || defaultBranchWidthRem}rem`}
		>
			<ScrollableContainer>
				<div class="card">
					<BaseBranch {projectId} base={$baseBranch} {selectedFiles} project={$project$} />
				</div>
			</ScrollableContainer>
			<Resizer
				viewport={rsViewport}
				direction="right"
				minWidth={320}
				on:width={(e) => {
					laneWidth = e.detail / (16 * $userSettings.zoom);
					lscache.set(laneWidthKey, laneWidth, 7 * 1440); // 7 day ttl
				}}
			/>
		</div>
		<div class="base__right">
			{#if selected}
				<FileCard
					conflicted={selected.conflicted}
					branchId={'blah'}
					file={selected}
					{projectPath}
					{selectedOwnership}
					isUnapplied={false}
					readonly={true}
					on:close={() => {
						const selectedId = selected?.id;
						selectedFiles.update((fileIds) => fileIds.filter((file) => file.id != selectedId));
					}}
				/>
			{/if}
		</div>
	</div>
{/if}

<style lang="postcss">
	.base {
		display: flex;
		flex-grow: 1;
		overflow-x: auto;
	}
	.base__left {
		display: flex;
		flex-grow: 0;
		flex-shrink: 0;
		overflow-x: hidden;
		position: relative;
	}
	.base__right {
		display: flex;
		overflow-x: auto;
		align-items: flex-start;
		padding: var(--size-12) var(--size-12) var(--size-12) var(--size-6);
		width: 50rem;
	}
	.card {
		margin: var(--size-12) var(--size-6) var(--size-12) var(--size-12);
		padding: var(--size-16);
	}
</style>
