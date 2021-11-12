<template>
    <tr :class="isSelected ? 'selected' : ''" @click="selectRow()">
        <td><i class="bi bi-folder me-1"></i>{{name}}</td>
        <td>Chemical search engine</td>
        <td>11.11.1111</td>
    </tr>
</template>

<script>
export default {
    name: 'StoredItem',
    props: {
        name: String,
        isSelected: Boolean
    },
    methods: {
        selectRow() {
            this.$emit("selectRow");
        }
    }
}
</script>