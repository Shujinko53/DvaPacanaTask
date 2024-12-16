<script setup>
defineProps({
	cardData: {
		type: Object,
		default: () => { }
	},
	active: {
		type: Boolean,
		default: false,
	},
})
</script>

<template>
	<div :class="['card', { 'card--active': active }]">
		<h3 class="card__title" v-html="cardData.name"></h3>
	</div>
</template>

<style lang="scss">
.card {
	display: flex;
	align-items: center;
	justify-content: center;
	padding: 1rem;
	border: 1px solid $black;
	transition: background-color .3s ease, color .3s ease, opacity .1s ease-in;
	cursor: pointer;

	&__title {
		font-size: 2rem;
		font-weight: 500;
	}

	@media (hover: hover) {
		&:hover {
			background-color: rgba($black, .2);
			color: rgba($black, .8);
		}
	}

	&:active {
		background-color: rgba($black, .1);
	}

	&--active {
		background-color: rgba($black, .1);
		color: rgba($black, .8);

		@media (hover: hover) {
			&:hover {
				animation: pulse 1s linear infinite;
			}
		}

	}

	@media screen and (max-width: 560px) {
		&__title {
			font-size: 1.6rem;
			white-space: nowrap;
		}
	}

	@keyframes pulse {
		0% {
			background-color: rgba($black, .1);
		}

		50% {
			background-color: rgba($black, .2);
		}

		100% {
			background-color: rgba($black, .1);
		}
	}
}
</style>
