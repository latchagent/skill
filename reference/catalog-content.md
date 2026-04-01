# Catalog: Content

Content generation capabilities are coming soon. Check back later or run `clawcard agent catalog --json` for the latest availability.

## Async Jobs & Artifacts

When async capabilities launch, they will return a `jobId` instead of immediate results.

Poll for completion:
```
clawcard agent jobs status <job-id> --json
```

When complete, download the artifact:
```
clawcard agent artifacts download <art-id> --json
```

List artifacts:
```
clawcard agent artifacts list --json
```

List jobs:
```
clawcard agent jobs list --json
```
