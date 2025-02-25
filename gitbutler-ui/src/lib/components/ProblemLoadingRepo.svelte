<script lang="ts">
	import DecorativeSplitView from './DecorativeSplitView.svelte';
	import ProjectSwitcher from './ProjectSwitcher.svelte';
	import RemoveProjectButton from './RemoveProjectButton.svelte';
	import { ProjectService, type Project } from '$lib/backend/projects';
	import Icon from '$lib/components/Icon.svelte';
	import { UserService } from '$lib/stores/user';
	import { getContextByClass } from '$lib/utils/context';
	import * as toasts from '$lib/utils/toasts';
	import { goto } from '$app/navigation';

	export let project: Project;
	export let error: any = undefined;

	const projectService = getContextByClass(ProjectService);
	const userService = getContextByClass(UserService);
	const user = userService.user;

	let loading = false;
	let deleteConfirmationModal: RemoveProjectButton;

	async function onDeleteClicked() {
		loading = true;
		try {
			deleteConfirmationModal.close();
			await projectService.deleteProject(project.id);
			toasts.success('Project deleted');
			goto('/');
		} catch (e) {
			console.error(e);
			toasts.error('Failed to delete project');
		} finally {
			loading = false;
			projectService.reload();
		}
	}
</script>

<DecorativeSplitView
	user={$user}
	imgSet={{
		light: '/images/img_repo-load-error-light.webp',
		dark: '/images/img_repo-load-error-dark.webp'
	}}
>
	<div class="problem" data-tauri-drag-region>
		<p class="problem__project text-bold"><Icon name="repo-book" /> {project?.title}</p>
		<p class="problem__title text-base-body-18 text-bold" data-tauri-drag-region>
			There was a problem loading this repo
		</p>

		<div class="problem__error text-base-body-12">
			<Icon name="error" color="error" />
			{error ? error : 'An unknown error occurred'}
		</div>

		<div class="remove-project-btn">
			<RemoveProjectButton
				bind:this={deleteConfirmationModal}
				projectTitle={project.title}
				isDeleting={loading}
				{onDeleteClicked}
			/>
		</div>

		<div class="problem__switcher">
			<ProjectSwitcher {project} />
		</div>
	</div>
</DecorativeSplitView>

<style lang="postcss">
	.problem__project {
		display: flex;
		gap: var(--size-8);
		align-items: center;
		line-height: 120%;
		color: var(--clr-theme-scale-ntrl-30);
		margin-bottom: var(--size-20);
	}

	.problem__title {
		color: var(--clr-theme-scale-ntrl-30);
		margin-bottom: var(--size-12);
	}

	.problem__switcher {
		text-align: right;
		margin-top: var(--size-24);
	}

	.problem__error {
		display: flex;
		color: var(--clr-theme-scale-ntrl-0);
		gap: var(--size-12);
		padding: var(--size-20);
		background-color: var(--clr-theme-err-container);
		border-radius: var(--radius-m);
		margin-bottom: var(--size-12);
	}

	.remove-project-btn {
		display: flex;
		justify-content: flex-end;
		padding-bottom: var(--size-24);
		border-bottom: 1px dashed var(--clr-theme-scale-ntrl-60);
	}
</style>
