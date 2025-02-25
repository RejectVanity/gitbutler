<script lang="ts">
	import Button from './Button.svelte';
	import Icon from './Icon.svelte';
	import InfoMessage from './InfoMessage.svelte';
	import Link from './Link.svelte';
	import { slide } from 'svelte/transition';
	import type { AuthService } from '$lib/backend/auth';

	export let authService: AuthService;
	export let projectId: string;
	export let remoteName: string | null | undefined;
	export let branchName: string | null | undefined;

	type Check = { name: string; promise: Promise<any> };
	$: checks = [] as Check[];

	$: errors = 0;
	$: loading = false;

	async function checkCredentials() {
		if (!remoteName || !branchName) return;
		loading = true;
		errors = 0;

		try {
			checks = [
				{ name: 'Fetch', promise: authService.checkGitFetch(projectId, remoteName) },
				{
					name: 'Push',
					promise: authService.checkGitPush(projectId, remoteName, branchName)
				}
			];
			await Promise.allSettled(
				checks.map((c) =>
					c.promise.catch((reason) => {
						++errors; // Shows error state as soon as any promise is rejected
						throw reason;
					})
				)
			);
		} finally {
			loading = false;
		}
	}

	export function reset() {
		checks = [];
	}
</script>

<div class="credential-check">
	{#if checks.length > 0}
		<div transition:slide={{ duration: 250 }}>
			<InfoMessage
				style={errors > 0 ? 'warn' : loading ? 'neutral' : 'success'}
				filled
				outlined={false}
			>
				<svelte:fragment slot="title">
					{#if loading}
						Checking git credentials …
					{:else if errors > 0}
						There was a problem with your credentials
					{:else}
						All checks passed successfully
					{/if}
				</svelte:fragment>
				<svelte:fragment slot="content">
					<div class="checks-list" transition:slide={{ duration: 250, delay: 1000 }}>
						{#each checks as check}
							<div class="text-base-body-12 check-result">
								<i class="check-icon">
									{#await check.promise}
										<Icon name="spinner" spinnerRadius={4} />
									{:then}
										<Icon name="success-small" color="success" />
									{:catch}
										<Icon name="error-small" color="error" />
									{/await}
								</i>{check.name}

								{#await check.promise catch err}
									- {err}
								{/await}
							</div>
						{/each}
					</div>
					{#if errors > 0}
						<div class="text-base-body-12 help-text" transition:slide>
							<span>
								Try another setting and test again?
								<br />
								Consult our
								<Link href="https://docs.gitbutler.com/troubleshooting/fetch-push">
									fetch / push guide
								</Link>
								for help fixing this problem.
							</span>
						</div>
					{/if}
				</svelte:fragment>
			</InfoMessage>
		</div>
	{/if}
	<Button wide icon="test" disabled={loading} on:click={checkCredentials}>
		{#if loading || checks.length == 0}
			Test credentials
		{:else}
			Re-test credentials
		{/if}
	</Button>
	<div class="disclaimer">
		To test the push command, we create an empty branch and promptly remove it after the check. <Link
			href="https://docs.gitbutler.com/troubleshooting/fetch-push">Read more</Link
		> about authentication methods.
	</div>
</div>

<style>
	.credential-check {
		display: flex;
		flex-direction: column;
		gap: var(--size-16);
	}

	.checks-list {
		display: flex;
		flex-direction: column;
		gap: var(--size-4);
		margin-top: var(--size-4);
	}

	.check-icon {
		display: flex;
		margin-top: 0.063rem;
	}

	.check-result {
		display: flex;
		gap: var(--size-6);
	}

	.help-text {
		margin-top: var(--size-6);
	}

	.disclaimer {
		color: var(--clr-theme-scale-ntrl-50);
		background: var(--clr-theme-container-pale);
		border-radius: var(--radius-m);
		background: var(--clr-theme-container-pale);
		padding: var(--size-10) var(--size-12);
	}
</style>
