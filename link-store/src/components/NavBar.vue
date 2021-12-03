<template>
    <nav class="navbar navbar-expand-lg navbar-light bg-light px-3 border-bottom">
        <a class="navbar-brand" href="/"><strong class="default-font">LinkStore</strong></a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>

        <div class="collapse navbar-collapse" id="navbarSupportedContent">
            <ul class="navbar-nav me-auto">
            </ul>
            <button class="btn btn-secondary mx-1 border" data-bs-toggle="modal" data-bs-target="#import-file-model">Import</button>
            <button class="btn btn-secondary mx-1 border" v-on:click="exportData()">Export</button>
        </div>
    </nav>
    <div class="modal fade" id="import-file-model" tabindex="-1">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Import all links</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <input class="form-control mb-2" type="file" placeholder="Name" @change="onFileChange">
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button>
                    <button type="button" class="btn btn-primary" data-bs-dismiss="modal" v-on:click="importData()">Save</button>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    name: 'NavBar',
    methods: {
        importData() {
            this.$emit("importData", this.importedFile);
        },
        onFileChange(event) {
            this.importedFile = event.target.files[0];
        },
        exportData() {
            this.$emit("exportData");
        }
    },
    emits: [
        "importData",
        "exportData"
    ],
    data() {
        return {
            importedFile: null
        }
    }
}
</script>