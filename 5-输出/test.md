---
created: 2024-02-15
updated: 2024-02-15
Type: knowledge
Status: ⌛️ 等待
tags:
---
## test

```dataviewjs
const summary = true
const includeSection = true
let taskList = dv.current().file.tasks
			 .where(t => !t.completed && dv.date(t.start) <= dv.date('today'))
taskList = taskList
	.sort(t => t.line)
	.sort(t => t.path)
if (summary) { dv.span("*" + taskList.length + " tasks*\n") }
if (includeSection) {
	const regex = /> ([^\]]+)/;
	let section = null
	let page = null
	let sectionTaskList = []
	for (let t of taskList) {
		if ((sectionTaskList.length > 0) && 
			((page != t.path) || (section != t.section.toString()))) {
			dv.taskList(sectionTaskList,false)
		}
		if (page != t.path) {
			dv.header(2,t.path.replace(/\.md$/, ''))
			page = t.path
		}
		if (section != t.section.toString()) {
			section = t.section.toString()
			dv.header(3,  section.match(regex)[1])
			sectionTaskList = []
		}
		sectionTaskList = sectionTaskList.concat(t)
	}
	dv.taskList(sectionTaskList,false)
} else {
	dv.taskList(taskList,true)  // true = divide by file
}
``` 

```dataview
table without id 
	file.link as 日期,
	dateformat(created, "ccc") as 星期, 
	L.text as 总结, 
	L.link as 链接
from "0-复盘/日报"
flatten file.lists as L
where
	!L.parent and
	meta(L.section).subpath = "📑 Notes"
sort file.name desc,L.text desc
```