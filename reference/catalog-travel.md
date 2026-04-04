# Catalog: Travel

All powered by StableTravel (Amadeus).

## How to pay

```
clawcard agent pay <slug> '<json>' --json
```

## Capabilities

### flight-search ($0.06)
Search for flight offers by origin, destination, and date.
```json
{"origin": "JFK", "destination": "LAX", "departureDate": "2026-05-01"}
```
Optional: `returnDate`, `adults`, `travelClass`, `nonStop`

### hotel-search ($0.06)
Search for hotel availability by city and dates.
```json
{"cityCode": "NYC", "checkInDate": "2026-05-01", "checkOutDate": "2026-05-03"}
```

More travel capabilities available — flight tracking, booking, activities, transfers.

Browse all: `clawcard agent catalog-search "travel" --json`
