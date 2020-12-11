<template>
    <div>
        <button v-on:click="showAddForm = !showAddForm" >+</button>
        <div v-if="showAddForm" >
            <form @submit.prevent="onSubmit" >
                <input type="text" v-model="addText" placeholder="введите описание" >
                <input type="text" v-model="addSum" placeholder="введите сумму" >
                <select v-model="addType" >
                    <option value="HIGH_LEVEL_TYPE_COSTS" >расход</option>
                    <option value="HIGH_LEVEL_TYPE_INCOME" >доход</option>
                </select>
                <button type="submit" >добавить</button>
            </form>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            addText: "",
            addSum: "",
            addType: "HIGH_LEVEL_TYPE_COSTS",
            showAddForm: false
        }
    }, 
    methods: {
        onSubmit() {
            if (this.addText.trim()) {
                const newSpend = {
                    id: "", 
                    sum: this.addSum, 
                    highType: this.addType, 
                    concreteTypeId: 1, 
                    date: String(new Date), 
                    comment: this.addText.trim()
                }

                this.$emit("add-spend", newSpend);

                this.showAddForm = false;
                this.addText = "";
                this.addSum = "";
            }
        }
    }
}
</script>