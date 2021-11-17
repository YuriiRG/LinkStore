<template>
    <tr class="vimium-button" :class="isSelected ? 'selected' : ''" @click="selectRow()" @dblclick="dblclickHandler()">
        <td>
            <i class="bi bi-folder me-1" v-if="type=='category'"></i>
            <i class="bi bi-link me-1" v-else></i>
            {{name}}
        </td>
        <td>{{purpose}}</td>
        <td>{{creationDate}}</td>
    </tr>
</template>

<script>
export default {
    name: 'StoredItem',
    props: {
        isSelected: Boolean,
        type: String,
        purpose: String,
        name: String,
        link: String,
        creationDate: String
    },
    methods: {
        selectRow() {
            this.$emit("selectRow");
        },
        dblclickHandler() {
            if (this.type == 'link') {
                window.location = this.link;
            } else {
                this.$emit('changeDir');
            }
        }
    }
}
</script>