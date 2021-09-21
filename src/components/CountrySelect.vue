<template>
    <select 
    @change="onChange()"
    v-model="selected" 
    class="form-select mt-1 block w-full border p-3 rounded">
        <option value="0"> Select Country </option>
        <option v-for="country in countries" :key="country.ID">
        {{ country.Country }}
        </option>
    </select>
</template>

<script>
export default {
    name: "CountrySelect",
    props: ["countries"],
    data(){
        return{
            selected: 0
        }
    },
    methods: {
        onChange() {
            //when option is changed set the country
            const country = this.countries.find((item) => item.Country === this.selected)
            //send country to the @get-country in app.vue
            this.$emit("get-country", country)
        }
    }
}
</script>