<%*
let qcFileName = await tp.system.prompt("Note Title")
let titleName = qcFileName
await tp.file.rename('index')
let baseFolder = "/content/post/"
let staticFolder = "/static/covers"  // Ensure this is correct
let newFolder = `${baseFolder}/${titleName}/`
await tp.file.move(newFolder + `index`);
let title = titleName
let date = tp.date.now()
let description = titleName+' descriptions'
let image = 'cover.jpg'  // Use static folder path
let weight = '1'
setTimeout(() => { app.fileManager.processFrontMatter(tp.config.target_file, frontmatter =>{
    frontmatter["title"] = title;
    frontmatter["date"] = date;
    frontmatter["description"] = description;
    frontmatter["image"] = image;
    frontmatter["weight"] = 1;
}) }, 200)
-%>
