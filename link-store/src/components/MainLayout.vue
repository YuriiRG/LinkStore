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
            <div class="border action-icon-block btn" title="Remove folder" v-on:click="editLink()">
                <i class="bi bi-pencil-square action-icon"></i>
            </div>
        </div>
        <main class="flex-grow-1">
            <table style="width: 100%">
                <thead>
                    <tr>
                        <th style="width: 35%">Name</th>
                        <th style="width: 45%">Purpose</th>
                        <th style="width: 20%">Creation date</th>
                    </tr>
                </thead>
                <tbody>
                    <stored-item v-for="entry in linkList" :key="entry.name" :name="entry.name" :isSelected="entry.isSelected"/>
                </tbody>
            </table>
        </main>
    </div>
</template>
<script>
import StoredItem from './StoredItem.vue';
export default {
    components: { StoredItem },
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
        },
        editLink() {
            alert("removeFolder");
        }
    },
    data() {
        return {
            linkList: [
                {
                    name: "first",
                    isSelected: false
                },
                {
                    name: "second",
                    isSelected: true
                },
            ]
        }
    }
}
</script>
<style>
.action-icon {
    font-size: 3em;
}
div .action-icon-block {
    width: 4em;
    height: 4em;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 0%;
}
.action-icon-block:hover {
    background-color: #E8E9EB;
}
.action-sidebar {
    width: 4em;
}
main table {
    margin: 0;
}
main td, main th {
    border: 1px solid #dee2e6;
    padding-right: 0.25rem;
    padding-left: 0.25rem;
    font-weight: 400;
    font-size: 15pt;
}
main tbody tr.selected {
    background-color: var(--bs-light);
}
</style>
