<%*
let qcFileName = await tp.system.prompt("Note Title")
let titleName = qcFileName
await tp.file.rename('index')
let baseFolder = "/content/post/"
let staticFolder = "/static/covers"  // Ensure this is correct
let newFolder = `${baseFolder}/${titleName}/`
await tp.file.move(newFolder + `index`);
let title = titleName
let description = titleName+' descriptions'
let date = tp.date.now()
let categories = "categories"
let tags = "tags"
let image = "cover.jpg"  // Use static folder path
let weight = "1"
setTimeout(() => { app.fileManager.processFrontMatter(tp.config.target_file, frontmatter =>{
    frontmatter["title"] = title;
    frontmatter["date"] = date;
    frontmatter["description"] = description;
    frontmatter["tags"] = tags;
    frontmatter["categories"] = categories;
    frontmatter["image"] = image;
    frontmatter["weight"] = 1;
}) }, 200)
-%>
