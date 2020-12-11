<template>
    <div>
        <ul>
        <SpendItem
            v-for="(spend, index) in spends" 
            v-bind:spend="spend" 
            v-bind:index="index" 
            v-bind:key="spend.id" 
            v-on:rm-spend="rmSpend"
         />
         </ul>
    </div>
</template>

<script>
import SpendItem from '@/components/SpendItem';

export default {
    components: {
        SpendItem
    },
    props: ["spends"],
    methods: {
        rmSpend(id) {
            this.$emit("rm-spend", id);
        }
    }
}
</script>