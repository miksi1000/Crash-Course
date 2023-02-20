<template>
<div style="margin-top: 30px"></div>
<form @submit="onSubmit" class="formgroup-inline">
    <div class="field">
        <label for="name" class="p-sr-only">Name</label>
        <input v-tooltip.top="'Her kan du skrive din hunds navn'" v-model="name" id="name" type="text" class="text-base text-color surface-overlay p-2 border-1 border-solid surface-border border-round appearance-none outline-none focus:border-primary" placeholder="Name">
    </div>
    <div class="field">
        <label for="age" class="p-sr-only">Age</label>
        <input v-tooltip.top="'Her kan du skrive din hunds alder'" v-model="age" id="age" type="number" class="text-base text-color surface-overlay p-2 border-1 border-solid surface-border border-round appearance-none outline-none focus:border-primary" placeholder="Age">
    </div>
    <div class="field">
        <label for="url" class="p-sr-only">URL</label>
        <input v-tooltip.top="'Her kan du indsætte et billede af din hund'" v-model="url" id="url" type="text" class="text-base text-color surface-overlay p-2 border-1 border-solid surface-border border-round appearance-none outline-none focus:border-primary" placeholder="Image URL">
    </div>
    <button v-tooltip.right="'Tryk for at gemme'" type="submit" class="text-white bg-primary-500 border-primary-500 px-3 py-2 text-base border-1 border-solid border-round cursor-pointer transition-all transition-duration-200 hover:bg-primary-600 hover:border-primary-600 active:bg-primary-700 active:border-primary-700">Save</button>
</form>


</template>

<script>
export default {
    name: "AddPet",
    data(){
        return {
            name: "",
            age: "",
            url: "",
            // isFavorite: false,
        }
    },
    emits: ["add-pet"],
    methods: {
        onSubmit(e) {
            e.preventDefault();

            const newPet = {
                // id: Math.random(),
                name: this.name,
                age: this.age,
                url: this.url,
                isFavorite: false
            }

            // console.log("new pet", newPet);
            this.$emit("add-pet", newPet)

            this.name = "";
            this.age = "";
            this.url = "";

        }
    }
}
</script>

<style>

</style>