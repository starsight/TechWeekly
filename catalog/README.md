# 使用方法
本统计工具基于obsidian dataview插件，使用其中的dataviewjs实现对横向的模块信息（相对于纵向的时间顺序周报而言）汇总。

以下source code来自于obsidian论坛`https://forum-zh.obsidian.md/t/topic/3748/8` 。基于该汇总信息


# Source Code
```
//输入目标小标题（含#），例如：#### 项目进度条
const header = '#### 项目进度条'

// 按【路径或文件夹、文件名、标签】筛选并按修改时间降序排列
const pages = dv.pages('"00数据管理" or ""').filter(p => p.file.name.includes("") && !p.file.path.includes("template")).filter(p => p.file.name.includes("") || p.file.name.includes("")).sort(p=>p.file.mtime,"desc");

// This regex will return text from the Summary header, until it reaches
// the next header, a horizontal line, or the end of the file
const regex = new RegExp(`\n${header}\r?\n(.*?)(\n#+ |\n---|$)`, 's')

for (const page of pages) {
    const file = app.vault.getAbstractFileByPath(page.file.path)
    // Read the file contents
    const contents = await app.vault.read(file)
    // Extract the summary via regex
    const summary = contents.match(regex)
    //显示全部包括空结果if (summary) {
    //不显示空结果if (summary && summary[1].trim()) {
    if (summary && summary[1].trim()) {
        // Output the header and summary
        dv.header(2, page.file.link)
        //或者dv.header(2, '[[' + file.basename + ']]')
        dv.paragraph(summary[1].trim())
    }
}
```
