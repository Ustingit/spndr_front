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
                <select v-model="subType" v-if="addType">
                     <option v-for="(item, index) in list[addType]" :value="item.id" :key="index">
                        {{ item.name }}
                    </option>
                </select>
                <button type="submit" >добавить</button>
            </form>
        </div>
    </div>
</template>

<script>
export default {
    props: ["outcomeSubTypes", "incomeSubTypes"],
    mounted() {
        console.log("mounted");
        console.log(this.outcomeSubTypes);
        console.log(this.incomeSubTypes);
        console.log("end");
    },
    data() {
        return {
            addText: "",
            addSum: "",
            addType: "HIGH_LEVEL_TYPE_COSTS",
            subType: null,
            subTypeOptions: this.subTypeByType,
            showAddForm: false,
            list: {
                'HIGH_LEVEL_TYPE_COSTS': this.outcomeSubTypes,
                'HIGH_LEVEL_TYPE_INCOME': this.incomeSubTypes
            }
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
    },
    computed: {
        subTypeByType() {
            console.log("0");
            if (this.addType === "HIGH_LEVEL_TYPE_COSTS") {
                console.log("1", this.outcomeSubTypes);
                return this.outcomeSubTypes;
            } else {
                console.log("2", this.incomeSubTypes);
                return this.incomeSubTypes;
            }
        }
    }
}
</script>