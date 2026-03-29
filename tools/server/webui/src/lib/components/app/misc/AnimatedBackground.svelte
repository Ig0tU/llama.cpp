<script lang="ts">
	let { class: className = '' } = $props();
</script>

<div class="animated-bg-container {className}">
	<div class="gradient-bg">
		<svg xmlns="http://www.w3.org/2000/svg" class="hidden">
			<defs>
				<filter id="goo">
					<feGaussianBlur in="SourceGraphic" stdDeviation="10" result="blur" />
					<feColorMatrix
						in="blur"
						mode="matrix"
						values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 18 -8"
						result="goo"
					/>
					<feBlend in="SourceGraphic" in2="goo" />
				</filter>
			</defs>
		</svg>
		<div class="gradients-container">
			<div class="g1"></div>
			<div class="g2"></div>
			<div class="g3"></div>
			<div class="g4"></div>
			<div class="g5"></div>
		</div>
	</div>
</div>

<style>
	.animated-bg-container {
		position: fixed;
		top: 0;
		left: 0;
		width: 100vw;
		height: 100vh;
		z-index: -1;
		overflow: hidden;
		background: #0a0a0a; /* Dark charcoal */
	}

	.gradient-bg {
		width: 100%;
		height: 100%;
		position: relative;
		filter: url(#goo) blur(40px);
	}

	.gradients-container {
		width: 100%;
		height: 100%;
	}

	.gradients-container > div {
		position: absolute;
		width: 100%;
		height: 100%;
		top: 0;
		left: 0;
		background: radial-gradient(circle at center, var(--color) 0, rgba(0, 0, 0, 0) 50%) no-repeat;
		mix-blend-mode: hard-light;
		opacity: 0.4;
	}

	.g1 {
		--color: rgba(50, 50, 80, 0.6);
		animation: moveVertical 30s ease infinite;
	}

	.g2 {
		--color: rgba(60, 30, 60, 0.6);
		animation: moveInCircle 20s reverse infinite;
		transform-origin: calc(50% - 400px);
	}

	.g3 {
		--color: rgba(30, 60, 80, 0.6);
		animation: moveInCircle 40s linear infinite;
		top: calc(50% - 50%);
		left: calc(50% - 50%);
		transform-origin: calc(50% + 400px);
	}

	.g4 {
		--color: rgba(80, 50, 50, 0.6);
		animation: moveHorizontal 40s ease infinite;
		opacity: 0.5;
	}

	.g5 {
		--color: rgba(60, 60, 60, 0.8);
		animation: moveInCircle 25s ease infinite;
		width: 100%;
		height: 100%;
		top: -50%;
		left: -50%;
		transform-origin: calc(50% - 800px) calc(50% + 200px);
	}

	@keyframes moveInCircle {
		0% {
			transform: rotate(0deg);
		}
		50% {
			transform: rotate(180deg);
		}
		100% {
			transform: rotate(360deg);
		}
	}

	@keyframes moveVertical {
		0% {
			transform: translateY(-50%);
		}
		50% {
			transform: translateY(50%);
		}
		100% {
			transform: translateY(-50%);
		}
	}

	@keyframes moveHorizontal {
		0% {
			transform: translateX(-50%) translateY(-10%);
		}
		50% {
			transform: translateX(50%) translateY(10%);
		}
		100% {
			transform: translateX(-50%) translateY(-10%);
		}
	}

	.hidden {
		display: none;
	}
</style>
