<template>
    <div class="d-flex flex-row flex-grow-1">
        <div class="bg-light border-end action-sidebar">
            <div class="border action-icon-block" title="Add new link">
                <i class="bi bi-file-earmark-plus action-icon"></i>
            </div>
            <div class="border action-icon-block" title="Add new folder">
                <i class="bi bi-folder-plus action-icon"></i>
            </div>
        </div>
        <main class="flex-grow-1">
            Main table
        </main>
    </div>
</template>
<script>
export default {
    name: 'MainLayout'
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