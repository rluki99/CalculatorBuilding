<script setup>
import data from './data/data.json'
import { ref, computed } from 'vue'
import Scope from '../src/components/Scope.vue'
import Section from '../src/components/Section.vue'
import Result from '../src/components/Result.vue'

const scopes = data.find((scope) => scope.type === 'scopeDefinition').items
const sections = data.find((section) => section.type === 'sectionDefinition').items
const showResult = ref(false)
const allClickedItems = ref([])

const clickedItem = (selectedItems) => {
	console.log(JSON.stringify(selectedItems), '-clicked Item which is selected');
	
	allClickedItems.value = selectedItems.map(x => x.name)
}
</script>

<template>
	<div class="container">
		<!-- <Scope :scope="scope"/> -->
		<p>Questions with * are required</p>
		<Section
			v-for="(section, index) in sections"
			:key="section.id"
			:section="section"
			:data-index="index"
			:sectionsLength="sections.length"
			:sections="sections"
			@allSelected="clickedItem" />
      <Result v-if="showResult" :clickedScopes="allClickedItems"/>
      <button @click="showResult=true">Calculate results</button>
	</div>
</template>

<style scoped lang="scss">
@import './assets/mixins';
@import './assets/variables';

p {
	text-align: center;
	margin-bottom: 10px;
	font-weight: bold;
}
.container {
	padding: 20px 20px;
}

button {
  background-color: $question-bgc;
  display: block;
  margin: 0 auto;
  padding: 10px 20px;
  border-radius: $border-10;
  border: none;
  font-weight: bold;
  font-size: 30px;
  color: $white;
  cursor: pointer;
  transition: background-color 0.5s;

  &:hover {
    background-color: $tiptool;
  }
}
</style>
