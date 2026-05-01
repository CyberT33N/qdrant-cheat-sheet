# qdrant-cheat-sheet






# Docker


## Kompletter Reset nur für Qdrant/Roo

### 1. Roo Code: Index zurücksetzen

In Roo Code öffnest du die **Codebase Indexing Settings** und nutzt:

```text
Clear Index Data
```

Laut Roo-Doku löscht das alle Daten aus der Qdrant-Collection und leert den lokalen File-Cache; es ist genau für einen vollständigen Re-Index gedacht und nicht rückgängig zu machen. ([Roo Code Docs][1])

Falls Qdrant wegen der defekten Collection gar nicht mehr startet, mach direkt mit Schritt 2 weiter.

### 2. Qdrant-Container entfernen

```bash
docker rm -f qdrant
```

### 3. Qdrant-Volume löschen

```bash
docker volume rm qdrant_data
```

Docker erlaubt das Löschen eines Volumes nicht, solange es noch von einem Container verwendet wird; deshalb zuerst `docker rm -f qdrant`, dann `docker volume rm qdrant_data`. ([Docker Documentation][2])

### 4. Optional prüfen, ob alles weg ist

```bash
docker ps -a | grep qdrant
docker volume ls | grep qdrant
```

Wenn nichts ausgegeben wird, sind Container und Volume weg.

### 5. Qdrant frisch starten

Nimm wieder den Roo-Doku-Befehl:

```bash
docker run -d \
  --name qdrant \
  --restart unless-stopped \
  -p 6333:6333 \
  -v qdrant_data:/qdrant/storage \
  qdrant/qdrant
```
