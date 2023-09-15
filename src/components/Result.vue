<script setup>
import { defineProps,computed } from 'vue';
const { clickedScopes } = defineProps(['clickedScopes'])

</script>

<template>
    <div class="result-container">
        <div class="result">
            <span class="scopes" v-for="(scope, index) in clickedScopes" :key="scope">
                {{ scope }} <span v-if="index < clickedScopes.length - 1">+ </span>
            </span>
            <hr class="line">
            <span class="hours">Work hours: </span>
            <h3 class="total">Total: zł gross</h3>
        </div>
    </div>
</template>

<style scoped lang="scss">
@import '../assets/mixins';
@import '../assets/variables';
.result-container {
	max-width: 1200px;
	width: 100%;
	margin: 0 auto;
	color: $white;
}

.result {
    background-color: $question-bgc;
	border-radius: 10px;
	margin-block: 20px;
    padding: 20px 50px;

    @include view(mobile) {
        padding-inline: 30px;
    }
}

.scopes {
    font-size: 28px;

    @include view(mobile) {
        font-size: 20px;
    }
}

.line {
    color: white;
    background-color: white;
    width: 100%;
    height: 2px;
    margin-block: 10px;
}

.hours {
    font-size: 18px;
}
.total {
    font-weight: bold;
    font-size: 30px;

    @include view(mobile) {
        font-size: 24px;
    }
}
</style>