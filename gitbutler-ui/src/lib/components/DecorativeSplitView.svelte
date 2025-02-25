<script lang="ts">
	import AccountLink from './AccountLink.svelte';
	import Icon from './Icon.svelte';
	import ImgThemed from './ImgThemed.svelte';
	import type { User } from '$lib/backend/cloud';

	export let user: User | undefined = undefined;
	export let showLinks: boolean = true;
	export let imgSet: { light: string; dark: string } | undefined = undefined;
</script>

<div class="decorative-split-view">
	<div class="left-side hide-native-scrollbar" data-tauri-drag-region>
		<div class="left-side__content">
			<slot />
		</div>
	</div>

	<div class="right-side">
		<div class="right-side-wrapper" data-tauri-drag-region>
			{#if user}
				<div class="right-side__header">
					<div class="account-button">
						<AccountLink {user} pop />
					</div>
				</div>
			{/if}

			{#if imgSet}
				<div class="img-wrapper">
					<ImgThemed {imgSet} />
				</div>
			{/if}

			{#if showLinks}
				<div class="right-side__footer">
					<div class="right-side__links">
						<a
							class="right-side__link"
							target="_blank"
							href="https://docs.gitbutler.com/features/virtual-branches/branch-lanes"
						>
							<Icon name="docs" opacity={0.6} />
							<span class="text-base-14 text-semibold">GitButler Docs</span>
						</a>
						<a
							class="right-side__link"
							target="_blank"
							href="https://www.youtube.com/@gitbutlerapp"
						>
							<Icon name="video" opacity={0.6} />
							<span class="text-base-14 text-semibold">Watch tutorials</span>
						</a>
					</div>

					<!--  prettier-ignore -->
					<svg class="wordmark" width="110" height="24" viewBox="0 0 110 24" xmlns="http://www.w3.org/2000/svg" fill="currentColor">
                        <g opacity="0.6">
                        <path d="M9.81268 24C3.23275 24 0 18.8286 0 12.2571C0 5.34286 3.63327 0.228572 9.66964 0.228572C11.7008 0.228572 13.3029 0.771429 14.8192 2.05714C15.2483 2.45714 15.5916 2.34286 15.6488 1.8L15.7918 0.485715H16.4784L16.8504 8.57143H16.1924C15.3913 3.97143 13.4173 1.48572 10.156 1.48572C5.83611 1.48572 3.49023 5.88571 3.49023 11.8857C3.49023 17.9714 5.80751 22.6286 10.2418 22.6286C12.3874 22.6286 13.8751 21.4286 14.5903 20.3143V14.6857C14.5903 12.9429 13.9037 12.4286 9.24052 12.4286V11.7429H19.3679V12.4286C17.823 12.4286 17.5942 12.8571 17.5942 14.6857V20.1714C15.9635 22.4857 13.2171 24 9.81268 24Z" />
                        <path d="M23.5928 5.08571C22.5057 5.08571 21.7332 4.34286 21.7332 3.34286C21.7332 2.31429 22.5057 1.57143 23.5928 1.57143C24.6513 1.57143 25.4237 2.31429 25.4237 3.34286C25.4237 4.34286 24.6513 5.08571 23.5928 5.08571ZM25.1948 8.25714V20.6C25.1948 22.4286 25.4809 22.8571 27.226 22.8571V23.5429H20.4172V22.8571C22.1624 22.8571 22.4484 22.4286 22.4484 20.6V11.8C22.4484 9.88572 22.4198 9.48572 20.7605 10.1714L20.5031 9.51429L24.7085 7.82857L25.1948 8.25714Z" />
                        <path d="M36.2741 17.1429H37.0752C37.0752 21.6 36.1883 24 33.3847 24C31.1532 24 29.9803 22.6571 29.9803 18.6571V9.57143H27.9777V8.51429C30.0661 8.45714 31.3821 6.91429 31.6682 4.22857H32.6981V8.4H36.1311V9.57143H32.7553V17.9143C32.7553 20.4571 33.0128 22.1429 34.3574 22.1429C35.6447 22.1429 36.2741 20.5429 36.2741 17.1429Z" />
                        <path d="M46.737 23.5429H38.0686V22.8571C40.7292 22.8571 41.1011 22.3429 41.1011 20.6V3.62857C41.1011 1.88572 40.7292 1.37143 38.0686 1.37143V0.685716H47.0231C51.1999 0.685716 53.9749 2.51429 53.9749 6.34286C53.9749 9.51429 51.8865 11.3714 49.1115 11.7143V11.8571C52.6875 12.0857 55.0334 14.3143 55.0334 17.4C55.0334 21.9143 51.3143 23.5429 46.737 23.5429ZM44.2767 2.74286V11.1429C44.9919 11.2571 45.7357 11.2571 46.4223 11.2571C48.7682 11.2571 50.828 9.77143 50.828 6.45714C50.828 3.2 48.7396 1.85714 46.4795 1.85714C45.9359 1.85714 45.3352 1.88572 44.8488 2C44.4483 2.08571 44.2767 2.34286 44.2767 2.74286ZM44.2767 12.5429V21.4857C44.2767 22.1714 45.0205 22.3714 46.3651 22.3714C49.0543 22.3714 51.8293 21.3714 51.8293 17.4286C51.8293 13.4857 48.6537 12.4286 46.4509 12.4286C45.7071 12.4286 44.9919 12.4286 44.2767 12.5429Z" />
                        <path d="M65.8487 24L65.2766 23.5143L65.2193 21.0857L65.1335 21.0571C64.1608 22.6 62.7876 24 60.7564 24C58.7825 24 57.6381 22.4 57.6381 19.8857V11.8286C57.6381 9.97143 57.5523 9.62857 55.8072 10.1714L55.6069 9.54286L60.0126 8.11429L60.4131 8.48572V19.6C60.4131 21.2857 60.9281 22.0857 62.0152 22.0857C63.1595 22.0857 64.3039 21.1143 65.2193 19.8857V11.8286C65.2193 9.97143 65.1621 9.62857 63.3884 10.1714L63.1881 9.54286L67.6224 8.11429L67.9944 8.48572V20.1429C67.9944 22.0571 68.0516 22.4286 69.8253 21.7714L70.0828 22.4286L65.8487 24Z" />
                        <path d="M78.6838 17.1429H79.4849C79.4849 21.6 78.598 24 75.7944 24C73.5629 24 72.39 22.6571 72.39 18.6571V9.57143H70.3874V8.51429C72.4758 8.45714 73.7918 6.91429 74.0779 4.22857H75.1078V8.4H78.5408V9.57143H75.165V17.9143C75.165 20.4571 75.4225 22.1429 76.7671 22.1429C78.0545 22.1429 78.6838 20.5429 78.6838 17.1429Z" />
                        <path d="M85.0338 0.4V20.6C85.0338 22.4286 85.2913 22.8571 87.0364 22.8571V23.5429H80.2276V22.8571C81.9727 22.8571 82.2588 22.4286 82.2588 20.6V4.08571C82.2588 2.25714 82.2588 1.88572 80.5423 2.74286L80.2562 2.11429L84.6047 0L85.0338 0.4Z" />
                        <path d="M97.8709 17.4286H98.5575C97.9567 22.0286 96.0114 24 93.3222 24C90.0894 24 88.0582 20.9429 88.0582 16.2286C88.0582 11.7143 90.0608 7.88571 93.4652 7.88571C96.8124 7.88571 98.3859 11.0286 98.3859 15.2H90.8333V15.2571C90.8333 19.1429 92.0348 21.8857 94.4093 21.8857C95.6967 21.8857 97.1843 20.8857 97.8709 17.4286ZM93.4652 9.08571C91.7487 9.08571 91.0335 11.1714 90.8619 14.0286H95.6395C95.6395 11.2857 95.1531 9.08571 93.4652 9.08571Z" />
                        <path d="M108.455 7.88571C109.456 7.88571 110 8.45714 110 9.4C110 10.5143 109.342 11.2286 108.598 11.2286C108.083 11.2286 107.854 11.0286 107.482 10.6286C107.111 10.2571 106.968 10.1429 106.624 10.1429C105.966 10.1429 105.194 11.0857 104.593 12.8286V20.4286C104.593 22.3714 105.022 22.8286 107.625 22.8286V23.5429H99.9012V22.8571C101.561 22.8571 101.847 22.4286 101.847 20.6V11.8C101.847 9.88572 101.847 9.48571 100.13 10.2L99.844 9.54286L102.133 8.62857L104.078 7.82857L104.593 8.25714V11H104.679C105.48 9.48571 106.882 7.88571 108.455 7.88571Z" />
                        </g>
                        </svg>
				</div>
			{/if}
		</div>
	</div>
</div>

<style lang="postcss">
	.decorative-split-view {
		cursor: default;
		user-select: none;
		display: flex;
		flex-grow: 1;
		background-color: var(--clr-theme-container-light);
	}

	.right-side {
		display: flex;
		flex-direction: column;
		position: relative;
	}

	.left-side {
		display: grid;
		align-items: center;
		justify-content: center;
		padding: var(--size-40) calc(var(--size-40) * 2);
		flex: 1;
		background-color: var(--clr-theme-container-light);
		overflow-y: auto;
	}

	.left-side__content {
		display: flex;
		flex-direction: column;
		width: 100%;
		max-width: 36rem;
	}

	/* RIGHT SIDE */

	.right-side {
		flex: 1;
		min-width: 20rem;
		background-color: var(--clr-theme-container-light);
		padding: var(--size-20) var(--size-20) var(--size-20) 0;
	}

	.right-side-wrapper {
		position: relative;
		overflow: hidden;
		display: flex;
		flex-direction: column;
		height: 100%;
		background-color: var(--clr-theme-scale-pop-80);
		border-radius: var(--size-8);
	}

	.right-side__header {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		display: flex;
		align-items: center;
		justify-content: flex-end;
		padding: var(--size-20);
	}

	.right-side__footer {
		position: absolute;
		bottom: 0;
		left: 0;
		width: 100%;
		display: flex;
		align-items: flex-end;
		justify-content: space-between;
		padding: var(--size-32);
	}

	.wordmark {
		position: absolute;
		color: var(--clr-theme-scale-pop-30);
		right: var(--size-32);
		bottom: var(--size-32);
	}

	.account-button {
		right: var(--size-32);
		top: var(--size-32);
	}

	/* links */

	.right-side__links {
		color: var(--clr-theme-scale-pop-20);
		display: flex;
		align-items: flex-start;
		flex-direction: column;
		gap: var(--size-4);
	}

	.right-side__link {
		display: flex;
		align-items: center;
		gap: var(--size-10);
		padding: var(--size-6);
		border-radius: var(--radius-m);
		transition: background-color var(--transition-fast);

		&:hover {
			background-color: color-mix(in srgb, var(--clr-theme-scale-pop-50), transparent 85%);
		}
	}

	.img-wrapper {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		width: 100%;
		max-width: 26.25rem;
		overflow: hidden;
	}
</style>
