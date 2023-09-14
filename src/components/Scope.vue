<script setup>
import { defineProps } from 'vue';

const {scope} = defineProps(['scope'])
</script>

<template>
<div class="scope-container">

</div>
</template>

<style scoped lang="scss">
@import '../assets/mixins';
@import '../assets/variables';
.scope-container {
	max-width: 1200px;
	width: 100%;
	margin: 0 auto;
	color: $white;
}
</style>