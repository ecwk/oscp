1. Host Discovery
2. Port Scanning
3. Recon
4. Initial Access
5. Enumeration/pivoting/lateralmovement
6. Privesc

## Recon
```dataview
TABLE port
FROM #tool/recon
```

# Forensics
```dataview
TABLE
FROM #tool/forensics 
```

## Utility
```dataview
TABLE file.tags AS Tags
FROM #tool 
WHERE contains(file.tags, "util")
```


## All
```dataview
TABLE
FROM #tool 
```