<a class="banner" href="https://docs.gitbutler.com/community/supporters" target="_blank">
	<div class="benner-content">
		<h4 class="benner-label text-base-14 text-bold">Thank you to all GitButler early supporters</h4>
		<i class="benner-arrow-wrap">
			<div class="benner-arrow-tail"></div>
			<svg
				viewBox="0 0 7 11"
				fill="none"
				xmlns="http://www.w3.org/2000/svg"
				class="benner-arrow-head"
			>
				<path
					d="M0.871094 1L5.00013 5.5L0.871094 10"
					stroke="currentColor"
					vector-effect="non-scaling-stroke"
				/>
			</svg>
		</i>
	</div>
	<img class="banner-img" src="/images/banners/support.webp" alt="" />
</a>

<style>
	.banner {
		display: flex;
		align-items: center;
		padding: var(--size-16);
		border-radius: var(--radius-m);
		background-color: #d7f2f1;

		&:hover {
			& .benner-arrow-wrap {
				width: 1rem;
			}

			& .benner-arrow-tail {
				width: 90%;
			}
		}
	}

	.benner-content {
		display: inline;
		color: #000;
	}

	.benner-label {
		display: inline;
	}

	.banner-img {
		width: 3.25rem;
		height: auto;
	}

	/* ARROW */

	.benner-arrow-wrap {
		position: relative;
		display: inline-flex;
		transform: translateY(0.07rem);
		width: 0.5rem;
		height: 0.7rem;
		transition: width 0.2s;
	}

	.benner-arrow-tail {
		position: absolute;
		top: 50%;
		transform: translateY(-50%);
		left: 0;
		width: 0%;
		height: 0.094rem;
		background-color: currentColor;
		transition: width 0.2s;
	}

	.benner-arrow-head {
		position: absolute;
		right: 0;
		top: 0;
		height: 100%;

		& path {
			stroke-width: 0.094rem;
		}
	}
</style>
