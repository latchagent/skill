# Catalog: GTM

All powered by Apollo.

## How to call

```
clawcard agent wallet send --url "https://www.clawcard.sh/api/catalog/<capability>" --method POST --body '<json>' --json
```

## Capabilities

### find-contacts ($0.03)
Find people by job title, company, location, seniority from a database of 270M+ contacts.
```json
{"company": "Apollo", "title": "VP of Sales", "location": "San Francisco"}
```
Additional params: `domain`, `person_seniorities[]`, `organization_num_employees_ranges[]`, `page`, `per_page`

### enrich-company ($0.02)
Get company data — funding, revenue, employee count, tech stack, social links.
```json
{"domain": "apollo.io"}
```
Also accepts: `name` (company name), `organization_ids[]`, `organization_locations[]`

### enrich-prospect ($0.02)
Enrich a person's data — title, company, email, LinkedIn, employment history.
```json
{"email": "tim@apollo.io"}
```
Also accepts: `name`, `first_name`/`last_name`, `domain`, `linkedin_url`, `organization_name`. More fields = better match.

### competitor-analysis ($0.02)
Get a company's active job postings — see where they're growing headcount.
```json
{"domain": "apollo.io"}
```
Also accepts: `organization_id` directly if you have it.

### company-news ($0.02)
Find news articles about companies — hires, funding, contracts, and more.
```json
{"domain": "apollo.io"}
```
Additional params: `categories[]` (hires/investment/contract), `published_at[min]`, `published_at[max]`, `page`, `per_page`
