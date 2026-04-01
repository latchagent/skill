# Credentials

Store and retrieve secrets. Use consistent lowercase naming.

Naming convention:
- `--service`: lowercase service name (openai, aws, stripe, vercel)
- `--key`: lowercase key type (api_key, secret_key, access_token, password)

List stored credentials:
```
clawcard agent creds --json
```

Store credential:
```
clawcard agent creds set --service <name> --key <key> --value <secret> --json
```

Retrieve credential:
```
clawcard agent creds get --service <name> --key <key> --json
```
