<script context="module">
	export async function load({ fetch }) {
		const res = await fetch('/events/events');
		if (res.ok) return { props: { categories: await res.json() } };
		return {
			status: res.status,
			error: new Error(),
		};
	}
</script>

<script>
	export let categories;
</script>

<svelte:head>
	<title>Events | Meraz '22</title>
</svelte:head>

<div class="parent">
	{#each categories as category}
		<h1><a name={category.name}>{category.name}</a></h1>
		<div class="container" id={category.name}>
			{#each category.events as event}
				<a
					class={`${event.imageSize} event-item`}
					href={`/events/${event.name}`}
					style="cursor: pointer;"
				>
					<img class="event-img" src={`assets/event-images/${event.image}`} alt="Event" />
					<span class="event-title">{event.name}</span>
				</a>
			{/each}
		</div>
	{/each}
</div>

<style lang="scss">
	.container {
		display: grid;
		grid-gap: 10px;
		grid-template-columns: repeat(auto-fit, minmax(130px, 1fr));
		grid-auto-rows: 150px;
		grid-auto-flow: dense;
	}
	.parent {
		width: 80%;
		margin: 75px 10%;
	}

	div:focus {
		outline: none;
	}

	.event-item {
		display: flex;
		justify-content: center;
		align-items: center;
		scroll-snap-align: start;
		scroll-margin-top: 75px;
		position: relative;
		transition: transform 0.2s ease-in-out;
	}
	// .event-item:hover {
	// 	transform: translateY(-10px);
	// }
	.event-title {
		font-family: 'Metal Mania', cursive;
		position: absolute;
		bottom: 10px;
		left: 10px;
		text-shadow: black 0px 0px 5px;
		// font-weight: bold;
		font-size: 1.4em;
	}
	h1 {
		margin-top: 2em;

		scroll-snap-align: start;
		scroll-margin-top: 75px;

		font-weight: normal;
		font-family: 'Metal Mania', cursive;
		color: transparent;
		background: -webkit-linear-gradient(gold, gold, red);
		background-clip: text;
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
	}
	.event-img {
		width: 100%;
		height: 100%;
		object-fit: cover;
		// border: gold 2px ridge;
		// border-radius: 10px;
		cursor: pointer;
	}
	$yellow: gold;

	$border-width: 2px;
	// max-width : 500px
	@media (max-width: 500px) {
		$border-width: 0px;
		.event-item {
			border-width: 2px;
			border-style: solid;
			border-image: linear-gradient(135deg, #ff2600, yellow, yellow, #ff2600) 1 round;
		}
	}

	.event-item {
		//   A duration of .5s in transition of text color (Effective when hovered)
		transition: 0.5s color ease-in-out;

		// Change text color to $yellow on hover
		&:hover {
			color: $yellow;
		}

		// Start ::after in same position as ::before
		&::before,
		&::after {
			box-sizing: inherit;
			content: '';
			position: absolute;
			border: $border-width solid transparent;
			border-radius: 0px;
			width: 0;
			height: 0;
			top: 0;
			left: 0;
		}

		//   Change dimensions on hovering
		&:hover::before,
		&:hover::after {
			width: 100%;
			height: 100%;
		}

		&::before,
		&::after {
			width: 0%;
			height: 0%;

			border-width: 0px;
			border-style: solid;
			border-image: linear-gradient(135deg, #ff2600, yellow, yellow, #ff2600) 1 round;
		}

		&::before {
			transition: width 0.25s ease-out 0.25s,
				// Width expands first
				height 0.25s ease-out,
				border-width 0.5s ease-out 0.25s;
		}

		&::after {
			transition: // Animate height first, then width
				height 0.25s ease-out 0.25s, width 0.25s ease-out, border-width 0.25s ease-out 0.25s;
		}
		// Change colors
		&:hover::before {
			border-width: $border-width $border-width 0px 0px;
			// -webkit-transition: border-radius 1s;
			// Commented for now transition
			transition: width 0.25s ease-out,
				// Width expands first
				height 0.25s ease-out 0.25s;
			//   border-radius 0.6s; // And then height
		}

		&:hover::after {
			border-width: 0px 0px $border-width $border-width;
			transition: // Animate height first, then width
				height 0.25s ease-out, width 0.25s ease-out 0.25s;
		}
	}

	.horizontal {
		grid-column: span 2;
	}

	.vertical {
		grid-row: span 2;
	}

	.big {
		grid-column: span 2;
		grid-row: span 2;
	}
</style>
