---
tags:
  - Area
Status: 🟨
CreateTime:
---
# Projects
## Not Started🟥
```dataview
table Status,Deadline
from [[]] and #Project 
where contains(Status,"🟥")
```
## In Progress🟨
```dataview
table Status,Deadline
from [[]] and #Project 
where contains(Status,"🟨")
```
## Finished 🟩
```dataview
table Status,Deadline
from [[]] and #Project 
where contains(Status,"🟩")
```
