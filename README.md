# Obsidian-Dataview-folder-index-page
JavaScript code for Obsidian Dataview plugin for folder index page:

````Bash
```dataviewjs
// ===== SETTINGS ===== //
// true or false //
const settings = {
    folder: "",                  // Folder to scan (leave blank "" to use the current folder)
    showFolderHeader: true,          // Show folder title (#### Folder)
    showFileList: true,              // Show file list
    sortFiles: true,                 // Sort files alphabetically
    groupBySubfolder: true,          // Group by subfolders
    divider: "---",                  // Separator between groups (can be changed to "***" or "")
};

// ===== КОД (можно не трогать) ===== //
const targetFolder = settings.folder || dv.current().file?.folder || "";
const pages = dv.pages()
    .where(p => p.file.folder === targetFolder || p.file.folder.startsWith(targetFolder + "/"));

// Группировка
const groups = {};
for (const p of pages) {
    const folder = settings.groupBySubfolder ? p.file.folder : targetFolder;
    if (!groups[folder]) groups[folder] = [];
    groups[folder].push(p);
}

// Вывод
for (const folder of Object.keys(groups).sort()) {
    if (settings.showFolderHeader && folder !== targetFolder) {
        dv.header(4, folder);
    }

    if (settings.showFileList) {
        const files = settings.sortFiles 
            ? groups[folder].sort((a, b) => a.file.name.localeCompare(b.file.name)) 
            : groups[folder];
        dv.list(files.map(f => f.file.link));
    }

    if (settings.divider) {
        dv.span(settings.divider);
    }
}
```
````

## Important Requirements and Instructions

### Prerequisites

To use this functionality, you need to have the **Dataview plugin** installed and enabled in **Obsidian**.
### How to Use

1. **Install Dataview Plugin**:
* Go to Obsidian settings
* Navigate to Community Plugins
* Find and enable the Dataview plugin

2. **Implement the Code**:
* Create a new Obsidian page or open an existing one
* Copy and paste the provided code into the page
* After inserting the code, switch to **Preview Mode** to see the results

### Configuration

### Settings at the Top — Customize Parameters to Your Needs:

* **Folder** — specify your folder path (or leave it as "" for the current folder).
* **ShowFolderHeader** — if set to false, removes the #### Folder headers.
* **ShowFileList** — if set to false, hides the file list (only separators will remain).
* **SortFiles** — if set to false, files are displayed in the Obsidian order (no sorting applied).
* **GroupBySubfolder** — if set to false, all files are displayed in a single group without subfolder separation.
* **Divider** — can be replaced with "***", "" (to remove lines), or your custom symbol.

### Usage Examples:

* Show everything in the current folder → **folder: ""**.
* Display without subfolders → **groupBySubfolder: false**.
* Only folder headers without files → **showFileList: false**.
