---
type: index
---

#index 

```dataview
TABLE WITHOUT ID rows.file.link AS Name, ports AS Ports
WHERE ports > 0 OR ports[0] > 0
GROUP BY ports
```
