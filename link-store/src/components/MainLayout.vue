<template>
    <div class="d-flex flex-row flex-grow-1">
        <div class="bg-light border-end action-sidebar">
            <div class="border action-icon-block btn" title="Add new link" data-bs-toggle="modal" data-bs-target="#new-link-model">
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
                    <stored-item v-for="entry in onlyFolders" :key="entry.name" 
                    @selectRow="selectRow(linkList.indexOf(entry))"
                    @changeDir="changeDir(entry)"
                    :name="entry.name"
                    :isSelected="entry.isSelected"
                    :purpose="entry.purpose"
                    :type="entry.type"
                    :link="entry.link"/>
                    
                    <stored-item v-for="entry in onlyLinks" :key="entry.name" 
                    @selectRow="selectRow(linkList.indexOf(entry))"
                    @changeDir="changeDir(entry)"
                    :name="entry.name"
                    :isSelected="entry.isSelected"
                    :purpose="entry.purpose"
                    :type="entry.type"
                    :link="entry.link"/>
                </tbody>
            </table>
        </main>
    </div>
    <div class="modal fade" id="new-link-model" tabindex="-1">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">New link</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <input class="form-control mb-2" placeholder="Name" v-model="newLinkData.name">
                    <input class="form-control mb-2" placeholder="Purpose" v-model="newLinkData.purpose">
                    <input class="form-control" placeholder="Link" v-model="newLinkData.link">
                    
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button>
                    <button type="button" class="btn btn-primary" data-bs-dismiss="modal" v-on:click="newLink()">Save</button>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import StoredItem from './StoredItem.vue';
export default {
    components: { StoredItem },
    name: 'MainLayout',
    methods: {
        selectRow(index) {
            for (let i = 0; i < this.linkList.length; i++) {
                this.linkList[i].isSelected = false;
            }
            this.linkList[index].isSelected = true;
        },
        newLink() {
            let isLink = /^(http:|https:)\/\//;
            if (!isLink.test(this.newLinkData.link)) {
                this.newLinkData.link = "http://" + this.newLinkData.link;
            }
            this.linkList.push({
                name: this.newLinkData.name,
                purpose: this.newLinkData.purpose,
                link: this.newLinkData.link,
                type: "link"
            });
            this.newLinkData = {
                name: "",
                purpose: "",
                link: ""
            }
        },
        changeDir(entry) {
            this.currentFolder = entry;
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
    computed: {
        onlyFolders: function() {
            let currentFolders = this.linkList;
            currentFolders = currentFolders.filter(c => c.type == "category");
            return currentFolders;
        },
        onlyLinks: function() {
            let currentFolders = this.linkList;
            currentFolders = currentFolders.filter(c => c.type == "link");
            return currentFolders;
        },
        linkList: function() {
            if (this.currentFolder != null) {
                return this.currentFolder.children;
            } else {
                return this.linkTree;
            }
            
        },
    },
    data() {
        return {
            linkTree: [
                {
                    type: "category",
                    name: "maincat",
                    isSelected: false,
                    children: [
                        {
                            type: "link",
                            name: "first",
                            isSelected: false,
                            purpose: "test link",
                            link: "https://example.com",
                        },
                        {
                            type: "category",
                            name: "second",
                            isSelected: false,
                            children: []
                        }
                    ]
                },
            ],
            currentFolder: null,
            newLinkData: {
                name: "",
                purpose: "",
                link: ""
            }
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
    border-spacing: 0;
    border-collapse: separate;
    border-bottom: 1px solid #dee2e6;
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
