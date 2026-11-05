```dataviewjs
const folder = "System Design";
const pages = dv.pages();
const results = [];

for (const page of pages) {
    // Source note must be outside the folder
    if (page.file.path.startsWith(folder + "/")) continue;

    for (const link of page.file.outlinks) {
        const target = app.metadataCache.getFirstLinkpathDest(
            link.path,
            page.file.path
        );

        if (!target) continue;

        // Destination note must be inside the folder
        if (target.path.startsWith(folder + "/")) {
            results.push([
                page.file.link,
                dv.fileLink(target.path, false, link.display)
            ]);
        }
    }
}

dv.table(
    ["Source (Outside)", "Incoming Link (Inside)"],
    results
);
```
