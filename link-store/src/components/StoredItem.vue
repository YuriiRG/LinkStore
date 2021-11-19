<template>
    <tr class="vimium-button"  @click="dblclickHandler()">
        <td>
            <div class="d-flex flex-row" style="width: 100%;">
                <div @click.stop="selectRow()" class="flex-center">
                    <div class="vimium-button border rounded" :class="isSelected ? 'selected' : ''">
                        
                    </div>
                </div>
                <div class="flex-grow-1 ps-2">
                    <i class="bi bi-folder me-1" v-if="type=='category'"></i>
                    <i class="bi bi-link me-1" v-else></i>
                    {{name}}
                </div>
            </div>
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