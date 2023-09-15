<script setup>
import { defineProps, ref, watch, defineEmits} from 'vue'

const { section, sectionsLength, dataIndex } = defineProps(['section', 'sectionsLength', 'dataIndex'])
const selectedItems = ref([])

const toggleActive = (item) => {
	if (section.selectionType === 'multi') {
		const itemIndex = selectedItems.value.indexOf(item)
		if (itemIndex !== -1) {
			selectedItems.value.splice(itemIndex, 1)
		} else {
			selectedItems.value.push(item)
		}
	} else if (section.selectionType === 'single') {
		const existingSelection = selectedItems.value.find((selectedItem) =>
			section.items.some(
				(sectionItem) =>
					sectionItem.selectionType === 'single' && sectionItem !== item && selectedItems.value.includes(sectionItem)
			)
		);

		if (existingSelection) {
			selectedItems.value = [item];
		} else {
			if (selectedItems.value.includes(item)) {
				selectedItems.value = [];
			} else {
				selectedItems.value = [item];
			}
		}
	}


}

const isSelected = (item) => {
	return selectedItems.value.includes(item)
}

</script>

<template>
	<div class="questions-container">
		<div class="question">
			<div class="header">
				<span>Selection: {{ section.selectionType }}</span>
				<span>Question {{ dataIndex + 1 }}/{{ sectionsLength }}</span>
			</div>
			<h2>{{ section.name }}</h2>
			<div class="options-container">
				<div
					@click="() => { toggleActive(item); $emit('allSelected', selectedItems); }"
					:class="{ active: isSelected(item) }"
					class="option"
					v-for="(item, index) in section.items"
					:key="index">
					<span class="tool-tip">{{ item.description }}</span>
					<div class="texts">
						<h3 class="texts-name">{{ item.name }}</h3>
						<p class="texts-description">{{ item.description }}</p>
					</div>
					<button>
						<img :src="item.imgHref" :alt="`${item.imgHref} image`" />
					</button>
				</div>
			</div>
		</div>
	</div>
</template>

<style scoped lang="scss">
@import '../assets/mixins';
@import '../assets/variables';

.questions-container {
	max-width: 1200px;
	width: 100%;
	margin: 0 auto;
	color: $white;
}

.question {
	background-color: $question-bgc;
	border-radius: $border-10;
	padding-top: 20px;
	margin-bottom: 20px;
	padding-inline: 50px;

    @include view(mobile) {
        padding-inline: 30px;
    }
    .header {
        display: flex;
        justify-content: space-between;
    }

    span {
        font-weight: 600;
    }

	h2 {
		text-align: center;
		font-size: 28px;
	}
}

.options-container {
	display: flex;
	justify-content: center;
	align-items: center;
	padding-block: 20px 50px;
	gap: 150px;

	@include view(tablet) {
		flex-direction: column;
		gap: 10px;
	}

    @include view(mobile) {
        padding-bottom: 30px;
    }
}

.option {
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
	gap: 10px;
	padding: 20px;
	color: $black;
	background-color: $white;
	width: min-content;
	border-radius: $border-10;
	cursor: pointer;
	transition: background-color 0.5s, color 0.5s;
	position: relative;

	@include view(tablet) {
		flex-direction: row;
		justify-content: flex-start;
		width: 100%;
		padding-block: 10px;
		gap: 30px;
	}

	@include view(mobile) {
		gap: 10px;
        padding: 10px;
	}

	&:hover {
		background-color: $hover;
		color: $white;
	}

	.texts {
		@include view(tablet) {
			order: 1;
		}
	}

	.texts-name {
		text-align: center;
		font-weight: bold;

		@include view(tablet) {
			color: $black;
			text-align: left;
		}
	}

	.texts-description {
		display: none;

		@include view(tablet) {
			display: block;
			color: $black;
		}
	}

	button {
		border-radius: $border-circle;
		display: flex;
		align-items: center;
		justify-content: center;
		width: 120px;
		height: 120px;
		min-width: 120px;
		min-height: 120px;
		cursor: pointer;
		border: none;
		background-color: $black;
		position: relative;

		@include view(mobile) {
			width: 100px;
			height: 100px;
			min-width: 100px;
			min-height: 100px;
		}
	}

	img {
		width: 100%;
		border-radius: $border-circle;
	}

	.tool-tip {
		position: absolute;
		left: 50%;
		width: 300px;
		bottom: 0;
		transform: translate(-50%, 110%);
		background-color: $tiptool;
		color: $white;
		border-radius: $border-10;
		padding: 3px 5px;
		visibility: hidden;
		opacity: 0;
		transition: opacity 0.5s;
	}

	&:hover .tool-tip {
		visibility: visible;
		opacity: 1;

		@include view(tablet) {
			display: none;
		}
	}
}

.active {
	background-color: $active;
}
</style>
