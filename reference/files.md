# File Storage

Upload, list, and download files for your agent. Files are stored with a 30-day TTL.

## Commands

### List files
```
clawcard agent files list --json
```

### Upload a file
```
clawcard agent files upload /path/to/file.pdf --json
```

### Get file details (includes download URL)
```
clawcard agent files get <file-id> --json
```

## Notes

- Files expire after 30 days
- Supports any file type (images, PDFs, CSVs, JSON, etc.)
- Upload returns `{id, filename, blobUrl}` — save the `id` for later retrieval
- The `blobUrl` is a direct download link
