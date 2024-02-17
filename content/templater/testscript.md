<%*
let qcFileName = await tp.system.prompt("Note Title")
let titleName = qcFileName
await tp.file.rename('index')
let baseFolder = "/content/post/"
let newFolder = `${baseFolder}/${titleName}/` 
await tp.file.move(newFolder + `index`);
-%>