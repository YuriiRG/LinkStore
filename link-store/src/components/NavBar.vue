<template>
    <nav class="navbar navbar-expand-lg navbar-light bg-light px-3 border-bottom">
        <a class="navbar-brand" href="/">LinkStore</a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>

        <div class="collapse navbar-collapse" id="navbarSupportedContent">
            <ul class="navbar-nav me-auto">
            </ul>
            <button class="btn btn-secondary mx-1 border" v-on:click="importData()">Import</button>
            <button class="btn btn-secondary mx-1 border" v-on:click="exportData()">Export</button>
        </div>
    </nav>
</template>
<script>
export default {
    name: 'NavBar',
    methods: {
        importData() {
            alert("Imported");
        },
        exportData() {
            alert("Exported");
        }
    }
}
</script>