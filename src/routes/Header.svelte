<script>
	import { page } from '$app/state';
	import rug from '$lib/images/RUG_LOGO_Rood.svg';
	import conv from '$lib/images/converginHorizonsLogo.png';
</script>

<header>
		<div class="corner">
			<img src={rug} alt="Universiteitslogo" />
		</div>

		<h1>Like!</h1>

		<div class="corner right">
			<img src={conv} alt="ConvergingHorizonsLogo" />	
		</div>
	<h3>Achter de schermen van de (sociale) media</h3>
</header>


<style>
	header {
		display: block;
		justify-content: center;
	}
	h3 {
		text-align: center;
		margin-top:-0.5em;
		text-transform: uppercase;
	}

	.corner img {
		height: 3em;
		object-fit: contain;
		margin: 1rem;
		max-width: 27vw;
		/* filter: brightness(0)invert(1); */
		filter: brightness(0) invert(8%) sepia(97%) saturate(7488%) hue-rotate(12deg) brightness(92%) contrast(114%);
	}

	.corner {
		position: absolute;
		top: 0rem;
		left: 0;
		z-index: -1;
	}
	
	.right {
		left: auto;
		right: 0;
		z-index: -2;
	}

	@media print {
		.corner img {
			filter: brightness(0);
		}
	}

	h1 {
		height: 100%;
		text-align: center;
		padding: 0 0.5rem;
		color: var(--color-text);
		font-weight: 700;
		font-size: 2rem;
		text-transform: uppercase;
		letter-spacing: 0.1em;
		text-decoration: none;
		transition: color 0.2s linear;
		/* background-color: var(--color-bg-0); */
	}
</style>
