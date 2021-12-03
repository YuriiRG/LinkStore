<template>
    <nav-bar @exportData="exportData()" @importData="importData"/>
    <div class="d-flex flex-row flex-grow-1">
        <div class="bg-light border-end action-sidebar">
            <div class="border action-icon-block btn vimium-button" title="Add new link" data-bs-toggle="modal" data-bs-target="#new-link-model">
                <i class="bi bi-file-earmark-plus action-icon"></i>
            </div>
            <div class="border action-icon-block btn vimium-button" title="Add new folder" data-bs-toggle="modal" data-bs-target="#new-folder-model">
                <i class="bi bi-folder-plus action-icon"></i>
            </div>
            <div class="border action-icon-block btn" :class="isSomethingSelected ? 'vimium-button' : 'disabled'" title="Remove" v-on:click="removeLink()">
                <i class="bi bi-file-earmark-x action-icon"></i>
            </div>
            <div class="border action-icon-block btn" :class="isSomethingSelected ? 'vimium-button' : 'disabled'" title="Edit" data-bs-toggle="modal" data-bs-target="#edit-link-model" v-on:click="initEditLink()">
                <i class="bi bi-pencil-square action-icon"></i>
            </div>
        </div>
        <main class="flex-grow-1">
            <table style="width: 100%">
                <thead>
                    <tr>
                        <th style="width: 35%; padding: 0; margin: 0">
                            <div class="d-flex flex-row" style="width: 100%;">
                                <div v-on:click="goUpFolder()" class="flex-center border-end vimium-button"><i class="bi bi-arrow-up-short"></i></div>
                                <div class="flex-grow-1 ps-2">Name</div>
                            </div>
                        </th>
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
                    :link="entry.link"
                    :creationDate="entry.creationDate"/>
                    
                    <stored-item v-for="entry in onlyLinks" :key="entry.name" 
                    @selectRow="selectRow(linkList.indexOf(entry))"
                    @changeDir="changeDir(entry)"
                    :name="entry.name"
                    :isSelected="entry.isSelected"
                    :purpose="entry.purpose"
                    :type="entry.type"
                    :link="entry.link"
                    :creationDate="entry.creationDate"/>
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
    <div class="modal fade" id="new-folder-model" tabindex="-1">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">New category</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <input class="form-control mb-2" placeholder="Name" v-model="newFolderData.name">
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button>
                    <button type="button" class="btn btn-primary" data-bs-dismiss="modal" v-on:click="newFolder()">Save</button>
                </div>
            </div>
        </div>
    </div>
    <div class="modal fade" id="edit-link-model" tabindex="-1">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Edit selected link or folder</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <input class="form-control mb-2" placeholder="Name" v-model="editLinkData.name">
                    <template v-if="editLinkData.type == 'link'">
                        <input class="form-control mb-2" placeholder="Purpose" v-model="editLinkData.purpose">
                        <input class="form-control mb-2" placeholder="Link" v-model="editLinkData.link">
                    </template>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button>
                    <button type="button" class="btn btn-primary" data-bs-dismiss="modal" v-on:click="editLink()">Save</button>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import StoredItem from './StoredItem.vue';
import NavBar from './NavBar.vue';
export default {
    components: { StoredItem, NavBar },
    name: 'MainLayout',
    methods: {
        selectRow(index) {
            
            this.linkList[index].isSelected = !this.linkList[index].isSelected;

            this.isSomethingSelected = false;
            for (let i = 0; i < this.linkList.length; i++) {
                if (this.linkList[i].isSelected == true)
                    this.isSomethingSelected = true;
            }
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
                type: "link",
                creationDate: this.getCurrentDate()
            });
            this.newLinkData = {
                name: "",
                purpose: "",
                link: ""
            };
            this.updateDb();
        },
        changeDir(entry) {
            this.linkList.forEach(c => c.isSelected = false);
            this.currentPath.push(entry);
            this.isSomethingSelected = false;
        },
        newFolder() {
            this.linkList.push({
                name: this.newFolderData.name,
                type: "category",
                creationDate: this.getCurrentDate(),
                isSelected: false,
                children: []
            });
            this.newFolderData = {
                name: "",
                type: "category",
                isSelected: false,
                children: []
            };
            this.updateDb();
        },
        removeLink() {
            let selected = this.linkList.filter(c => c.isSelected);
            if (selected.length > 0) {
                selected.forEach((_, i) => this.linkList.splice(this.linkList.indexOf(selected[i]), 1));
            }
            this.updateDb();
        },
        initEditLink() {
            this.editLinkData.name = this.getSelectedEntry().name;
            this.editLinkData.type = this.getSelectedEntry().type;
            this.editLinkData.purpose = this.getSelectedEntry().purpose;
            this.editLinkData.link = this.getSelectedEntry().link;
        },
        getSelectedEntry() {
            if (this.linkList.filter(c => c.isSelected == true).length == 0) {
                return null;
            }
            return this.linkList.filter(c => c.isSelected == true)[0];
        },
        editLink() {
            this.getSelectedEntry().name = this.editLinkData.name;
            this.getSelectedEntry().type = this.editLinkData.type;
            this.getSelectedEntry().purpose = this.editLinkData.purpose;
            this.getSelectedEntry().link = this.editLinkData.link;
            this.updateDb();
        },
        goUpFolder() {
            this.linkList.forEach(c => c.isSelected = false);
            if (this.currentPath == [])
                return;
            this.currentPath.pop();
        },
        updateDb() {
            this.linkList.forEach(c => c.isSelected = false);
            let transaction = this.db.transaction("links", "readwrite");
            let links = transaction.objectStore("links");
            links.clear();
            for (let i = 0; i < this.linkTree.length; i++) {
                console.log("Updating db");
                console.log(JSON.parse(JSON.stringify(this.linkTree[i])));
                let request = links.add(JSON.parse(JSON.stringify(this.linkTree[i])));
                request.onsuccess = function() {
                    console.log("Successfully copied to db", request.result);
                };
                request.onerror = function() {
                    console.error("Error", request.error);
                };
            }
        },
        getCurrentDate() {
            let currentDate = new Date();
            let cDay = currentDate.getDate();
            let cMonth = currentDate.getMonth() + 1;
            let cYear = currentDate.getFullYear();
            return `${cYear}-${cMonth}-${cDay}`;
        },
        download(filename, text) {
            var element = document.createElement('a');
            element.setAttribute('href', 'data:text/plain;charset=utf-8,' + encodeURIComponent(text));
            element.setAttribute('download', filename);

            element.style.display = 'none';
            document.body.appendChild(element);

            element.click();

            document.body.removeChild(element);
        },
        exportData() {
            let transaction = this.db.transaction("links", "readonly");
            let links = transaction.objectStore("links");
            let request = links.getAll();
            let text = "";
            request.onsuccess = () => {
                text = JSON.stringify(request.result);
                this.download("linkstore-export.json", text);
            };
        },
        importData(file) {
            console.log(file);
            let fileReader = new FileReader();
            let importedDataArray = null;
            fileReader.readAsText(file);
            fileReader.onload = () => {
                importedDataArray = fileReader.result;
                importedDataArray = JSON.parse(importedDataArray);

                let transaction = this.db.transaction("links", "readwrite");
                let links = transaction.objectStore("links");
                links.clear();
                for (let i = 0; i < importedDataArray.length; i++) {
                    links.add(importedDataArray[i]);
                }
                window.location.reload();
            }
        }
    },
    beforeMount() {
        console.log("Start initing db");
        this.indexedDBRequest = indexedDB.open("LinkStoreDB", 1);

        let openRequest = this.indexedDBRequest;

        openRequest.onupgradeneeded = function() {
            let db = openRequest.result;
            if (!db.objectStoreNames.contains('links')) {
                db.createObjectStore('links', {keyPath: 'name'});
            }
        };

        openRequest.onerror = function() {
            console.error("Error", openRequest.error);
        };

        openRequest.onsuccess = () => {
            this.db = openRequest.result;
            console.log(this);
            let transaction = this.db.transaction("links", "readonly");
            let links = transaction.objectStore("links");
            let request = links.getAll();
            request.onsuccess = () => {
                console.log(request.result);
                this.linkTree = request.result;
            };
        };
    },
    computed: {
        onlyFolders: function() {
            let currentFolders = this.linkList;
            
            currentFolders = currentFolders.filter(c => c.type == "category");
            
            currentFolders = currentFolders.sort(function(a, b) {
                var nameA = a.name.toUpperCase();
                var nameB = b.name.toUpperCase();
                if (nameA < nameB) {
                    return -1;
                }
                if (nameA > nameB) {
                    return 1;
                }
                return 0;
            });

            return currentFolders;
        },
        onlyLinks: function() {
            let currentFolders = this.linkList;

            currentFolders = currentFolders.filter(c => c.type == "link");
            
            currentFolders = currentFolders.sort(function(a, b) {
                var nameA = a.name.toUpperCase();
                var nameB = b.name.toUpperCase();
                if (nameA < nameB) {
                    return -1;
                }
                if (nameA > nameB) {
                    return 1;
                }
                return 0;
            });

            return currentFolders;
        },
        currentFolder: function() {
            return this.currentPath[this.currentPath.length - 1];
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
            linkTree: [],
            newLinkData: {
                name: "",
                purpose: "",
                link: ""
            },
            newFolderData: {
                name: ""
            },
            editLinkData: {
                name: "",
                purpose: "",
                link: "",
                type: ""
            },
            currentPath: [],
            isSomethingSelected: false,
            indexedDBRequest: null,
            db: null
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
.flex-center {
    display: flex;
    justify-content: center;
    align-items: center;
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
    user-select: none;
    -webkit-user-select: none;
}
main tbody tr.selected {
    background-color: var(--bs-light);
}
main tbody div.vimium-button.selected {
    background-color: var(--bs-primary);
}
main tbody div.vimium-button {
    width: 1rem;
    height: 1rem;
}
</style>
