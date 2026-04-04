# Catalog: People & Companies

## How to pay

```
clawcard agent pay <slug> '<json>' --json
```

## Capabilities

### enrich-person ($0.01) — powered by Apollo
Get a person's title, company, email, LinkedIn, and employment history.
```json
{"name": "Tim Zheng", "domain": "apollo.io"}
```
Also accepts: `email`, `first_name`/`last_name`, `linkedin_url`, `organization_name`

### enrich-company ($0.01) — powered by Apollo
Get company data — funding, revenue, employee count, tech stack, social links.
```json
{"domain": "apollo.io"}
```

### find-contacts ($0.01) — powered by Apollo
Search 275M+ contacts by title, company, location, seniority.
```json
{"company": "Apollo", "title": "VP of Sales"}
```

### find-email ($0.02) — powered by Hunter
Find someone's email address by name and company.
```json
{"first_name": "Tim", "last_name": "Zheng", "domain": "apollo.io"}
```

### verify-email ($0.01) — powered by Hunter
Check if an email address is valid and deliverable.
```json
{"email": "tim@apollo.io"}
```

### tech-stack ($0.06) — powered by BuiltWith
Detect a website's tech stack — frameworks, analytics, CDNs, CMS.
```json
{"domain": "stripe.com"}
```

Browse all enrichment capabilities: `clawcard agent catalog-search "enrich" --json`
