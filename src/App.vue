<script setup>
import { ref, reactive } from 'vue';
import Card from '@/components/Card.vue';

// ----- Data -----
const leftList = [
	{
		"id": 1,
		"name": "Shoes 1"
	},
	{
		"id": 2,
		"name": "Shoes 2"
	},
	{
		"id": 3,
		"name": "Shoes 3"
	},
	{
		"id": 4,
		"name": "Shoes 4"
	},
	{
		"id": 5,
		"name": "T-shirt 1"
	},
	{
		"id": 6,
		"name": "T-shirt 2"
	},
	{
		"id": 7,
		"name": "T-shirt 3"
	},
	{
		"id": 8,
		"name": "T-shirt 4"
	}
];
const resultLeft = reactive([]);

const rightList = [
	{
		"id": 11,
		"name": "Jacket 1"
	},
	{
		"id": 12,
		"name": "Jacket 2"
	},
	{
		"id": 13,
		"name": "Jacket 3"
	},
	{
		"id": 14,
		"name": "Jacket 4"
	},
	{
		"id": 15,
		"name": "Hoodie 1"
	},
	{
		"id": 16,
		"name": "Hoodie 2"
	},
	{
		"id": 17,
		"name": "Hoodie 3"
	},
	{
		"id": 18,
		"name": "Hoodie 4"
	}
];

const leftListLimit = 6;

let resultRight = reactive({});

// ----- Methods -----

function clickLeftCard(card) {
	if (resultLeft.includes(card)) {
		removeLeftCard(card);
	} else if (!resultLeft.includes(card) && resultLeft.length < leftListLimit) {
		resultLeft.push(card);
	}

}

function removeLeftCard(card) {
	const targetIndex = resultLeft.findIndex(item => item.id === card.id);
	resultLeft.splice(targetIndex, 1);
}

function clickRightCard(card) {
	if (resultRight && resultRight.id === card.id) {
		clearObject(resultRight);
	} else {
		resultRight.id = card.id;
		resultRight.name = card.name;
	}
}

function clearObject(obj) {
	Object.keys(obj).forEach(key => {
		delete obj[key];
	});
}

function beforeLeave(el) {
	const { width, height, left } = el.getBoundingClientRect();

	Object.assign(el.style, {
		left: left + 'px',
		width: width + 'px',
		height: height + 'px',
	});
}

</script>

<template>
	<div class="Home-page">
		<div class="Home-page__results results">
			<div class="results__block-left">
				<TransitionGroup
					name="list"
					@before-leave="beforeLeave"
				>
					<Card
						v-for="card in resultLeft"
						:key="card.id"
						:card-data="card"
						@click="removeLeftCard(card)"
					/>
				</TransitionGroup>

				<div class="results__block-left__counter">
					<span class="number" :key="resultLeft.length">{{ resultLeft.length }}</span>
					<span>/</span>
					<span>{{ leftListLimit }}</span>
				</div>
			</div>

			<div class="results__block-right">
				<Transition name="fade">
					<Card v-if="resultRight" :key="resultRight.id" :card-data="resultRight" />
				</Transition>
			</div>
		</div>

		<div class="Home-page__content">
			<div class="Home-page__content-left">
				<Card
					v-for="card in leftList"
					:key="card.id"
					:card-data="card"
					:active="resultLeft.includes(card)"
					@click="clickLeftCard(card)"
				/>
			</div>

			<div class="Home-page__content-right">
				<Card
					v-for="card in rightList"
					:key="card.id"
					:card-data="card"
					:active="card.id === resultRight.id"
					@click="clickRightCard(card)"
				/>
			</div>
		</div>
	</div>
</template>

<style lang="scss">
.Home-page {
	position: relative;
	display: flex;
	flex-direction: column;
	row-gap: 5rem;
	min-height: 100vh;
	padding: 4rem 2rem;

	&__results {
		display: flex;
		justify-content: space-between;
		gap: 2rem;
	}

	.results {
		&__block-left {
			position: relative;
			display: grid;
			grid-template-columns: repeat(auto-fill, minmax(14rem, 1fr));
			grid-auto-rows: 14rem;
			gap: 1rem;
			width: calc(50% - 1rem);
			min-height: calc(14rem + 2rem + 2px);
			padding: 1rem;
			border: 1px solid $black;

			&__counter {
				position: absolute;
				bottom: -1rem;
				left: 0;
				display: flex;
				align-items: center;
				padding-left: 2rem;
				column-gap: 1rem;
				font-size: 1.8rem;
				transform: translateY(100%);

				.number {
					position: absolute;
					left: 0;
					animation: fontWeight .8s ease forwards;

					@keyframes fontWeight {
						0% {
							font-size: 1.8rem;
							font-weight: 400;
						}

						50% {
							font-size: 2.2rem;
							font-weight: 700;
						}

						100% {
							font-size: 1.8rem;
							font-weight: 400;
						}
					}
				}
			}
		}

		&__block-right {
			position: relative;
			width: 14rem;
			height: 14rem;
			padding: 1rem;
			border: 1px solid $black;

			.card {
				top: 0;
				left: 0;
				width: 100%;
				height: 100%;
				border: 0;
				pointer-events: none;
			}
		}
	}

	&__content {
		display: flex;
		gap: 2rem;
		flex-grow: 1;

		&-left,
		&-right {
			display: grid;
			grid-template-columns: repeat(auto-fill, minmax(14rem, 1fr));
			grid-auto-rows: 14rem;
			gap: 1rem;
			flex-grow: 1;
			padding: 2rem;
			border: 1px solid $black;
		}
	}

	@media screen and (max-width: 560px) {
		padding: 2 1.2rem;

		&__results {
			flex-direction: column;
			justify-content: flex-start;
			gap: 5rem
		}

		.results {
			&__block-left {
				grid-template-columns: repeat(auto-fill, minmax(10rem, 1fr));
				grid-auto-rows: 10rem;
				width: 100%;
			}

			&__block-right {
				width: 10rem;
				height: 10rem;
				cursor: default;
			}
		}

		&__content {
			flex-direction: column;

			&-left,
			&-right {
				grid-template-columns: repeat(auto-fit, minmax(10rem, 1fr));
				grid-auto-rows: 10rem;
			}
		}
	}
}
</style>
