# Catalog: Maps & Weather

## How to pay

```
clawcard agent pay <slug> '<json>' --json
```

## Capabilities

### geocode ($0.01) — powered by Google Maps
Convert an address to latitude/longitude coordinates.
```json
{"address": "1600 Amphitheatre Parkway, Mountain View, CA"}
```

### directions ($0.01) — powered by Google Maps
Get directions and travel time between two locations.
```json
{"origin": "New York, NY", "destination": "Boston, MA", "mode": "driving"}
```

### weather ($0.01) — powered by OpenWeather
Get current weather conditions for any location.
```json
{"lat": 40.7128, "lon": -74.0060}
```

More maps capabilities available — places search, distance matrix, elevation, reverse geocode, air quality, weather forecast.

Browse all: `clawcard agent catalog-search "maps" --json`
