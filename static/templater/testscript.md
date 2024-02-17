<%*
let qcFileName = await tp.system.prompt("Note Title")
let titleName = qcFileName
await tp.file.rename('index')
let baseFolder = "/content/post/"
let newFolder = `${baseFolder}/${titleName}/` 
await tp.file.move(newFolder + `index`);
let title = titleName
let description = ""
let date = ""
let image = ""
let weight = "1"
setTimeout(() => { app.fileManager.processFrontMatter(tp.config.target_file, frontmatter =>{ frontmatter["title"] = title; 
frontmatter["description"] = description;
frontmatter["date"] = date; 
frontmatter["image"] = image; 
frontmatter["weight"] = 1; 
}) }, 200)
-%>