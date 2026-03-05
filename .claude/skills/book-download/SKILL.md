```skill
---
name: book-download
description: Search and download books from shadow libraries (Anna's Archive, Libgen+). Use when asked to find or download books by title or ISBN.
allowed-tools: Bash(playwright-cli:*), Bash(curl.exe:*)
---

# Book Search & Download

## Libraries (check uptime at https://open-slum.org)
- **Anna's Archive**: `annas-archive.pk` or `annas-archive.gl`
- **Libgen+**: `libgen.gl` or `libgen.la`

## Search

```bash
# Anna's Archive (title or ISBN)
playwright-cli goto "https://annas-archive.pk/search?q=TITLE+OR+ISBN"
playwright-cli snapshot

# Libgen+ (title or ISBN)
playwright-cli goto "https://libgen.gl/index.php?req=QUERY&columns[]=t&columns[]=a&columns[]=s&columns[]=y&columns[]=p&columns[]=i&objects[]=f&topics[]=l&res=25&filesuns=all"
playwright-cli snapshot
```

Extract MD5 from snapshot:
```bash
Select-String -Path ".playwright-cli\page-*.yml" -Pattern "md5" | Select-Object -First 5 -ExpandProperty Line
```

## Download (Libgen)

Keys are time-limited — always fetch a fresh one:
```bash
# 1. Get fresh key
playwright-cli goto "https://libgen.gl/ads.php?md5=MD5HASH"
playwright-cli snapshot
Select-String -Path ".playwright-cli\page-*.yml" -Pattern "get\.php" | Select-Object -First 1 -ExpandProperty Line

# 2. Download (run in terminal so user sees progress)
curl.exe -L -o "output.pdf" --progress-bar "https://libgen.gl/get.php?md5=MD5HASH&key=KEY"
```

## Verify
```bash
Get-Item "output.pdf" | Select-Object @{N='SizeMB';E={[math]::Round($_.Length/1MB,1)}}
```
Expected size from Libgen listing — if file is smaller, key likely expired; get a new one and retry.
```
