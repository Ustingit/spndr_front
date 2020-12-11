<template>
    <li>
        <span>{{index + 1}})</span>
        <span><img width="26" :src="imgSrc" /></span>
        {{spend.sum}} - {{spend.comment}}
        <button v-on:click="$emit('rm-spend', spend.id)" >&times;</button>
    </li>
</template>

<script>
export default {
    props: ["index", "spend"],
    computed: {
        imgSrc() {
            return this.spend.highType === "HIGH_LEVEL_TYPE_COSTS" ? "img/outcome.png" : "img/income.png";
        }
    }
}
</script>

<style scoped>

    li {
        list-style-type: none;
    }

</style>