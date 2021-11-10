<template>
    <div class="d-flex flex-row flex-grow-1">
        <div class="bg-light border-end action-sidebar">
            <div class="border action-icon-block btn" title="Add new link" v-on:click="newLink()">
                <i class="bi bi-file-earmark-plus action-icon"></i>
            </div>
            <div class="border action-icon-block btn" title="Add new folder" v-on:click="newFolder()">
                <i class="bi bi-folder-plus action-icon"></i>
            </div>
            <div class="border action-icon-block btn" title="Remove link" v-on:click="removeLink()">
                <i class="bi bi-file-earmark-minus action-icon"></i>
            </div>
            <div class="border action-icon-block btn" title="Remove folder" v-on:click="removeFolder()">
                <i class="bi bi-folder-minus action-icon"></i>
            </div>
        </div>
        <main class="flex-grow-1">
            Main table
        </main>
    </div>
</template>
<script>
export default {
    name: 'MainLayout',
    methods: {
        newLink() {
            alert("newLink");
        },
        newFolder() {
            alert("newFolder");
        },
        removeLink() {
            alert("removeLink");
        },
        removeFolder() {
            alert("removeFolder");
        }
    }
}
</script>
<style scoped>
.action-icon {
    font-size: 3em;
}
.action-icon-block {
    width: 4em;
    height: 4em;
    display: flex;
    justify-content: center;
    align-items: center;
}
.action-sidebar {
    width: 4em;
}
</style>