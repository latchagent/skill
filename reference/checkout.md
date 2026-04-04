# Browser Checkout

Use the `clawcard-browser` MCP server to navigate websites, fill checkout forms, and pay with virtual cards.

## Flow

**1. Check existing cards:**
```
clawcard agent cards --json
```
Look for an open `merchant_locked` card for this merchant. If found, skip to step 3.

**2. Create a card (if needed):**
```
clawcard agent cards create --amount <cents> --type merchant_locked --memo "description" --json
```
Beta limits: $50 max per card, 2 cards per month.

**3. Get billing address:**
```
clawcard agent billing-address --json
```

**4. Launch browser and navigate:**
Use the `launch_browser` MCP tool to open the checkout page.

**5. Fill the form:**
- Use `snapshot` to see the page structure with @e1, @e2 refs
- Use `fill` to type into non-payment fields (email, name, etc.)
- Use `click` to interact with buttons, dropdowns, checkboxes
- Use `detect_checkout` to confirm payment form is present
- Use `fill_checkout` with your card ID and billing details — this fills ALL payment fields securely

**6. Review and submit:**
- Use `snapshot` to verify fields are filled
- Use `click` to submit the payment form
- **ALWAYS ask the user before submitting a payment**

## MCP Tools

| Tool | Purpose |
|---|---|
| `launch_browser` | Open browser with ClawCard Pay extension at a URL |
| `navigate` | Go to a different URL |
| `snapshot` | Get accessibility tree with element refs (@e1, @e2...) |
| `click` | Click an element by ref |
| `fill` | Type into a field by ref |
| `detect_checkout` | Check if page is a checkout — returns confidence and amount |
| `fill_checkout` | Fill card + billing fields securely |
| `screenshot` | Capture the current page |
| `close_browser` | Close browser session |

## fill_checkout Parameters

| Param | Required | Description |
|---|---|---|
| `cardId` | Yes | ClawCard card ID |
| `email` | No | Email for checkout |
| `name` | No | Cardholder name |
| `phone` | No | Phone number |
| `addressLine1` | No | Street address |
| `addressCity` | No | City |
| `addressState` | No | State |
| `addressZip` | No | ZIP code |
| `addressCountry` | No | Country code (default: US) |

## Security

- Card details (PAN, CVV, expiry) are sent directly to the browser extension
- The LLM **never sees** card numbers — only the last four digits are returned
- The extension handles Stripe iframes, React inputs, and standard forms
- A branded 🦞 ClawCard Pay badge appears on checkout pages

## Example

```
# 1. Create card
clawcard agent cards create --amount 2500 --type merchant_locked --memo "Namecheap domain" --json
# → { "id": "card_abc", "lastFour": "4242", ... }

# 2. Get billing address
clawcard agent billing-address --json
# → { "name": "Agent-001", "line1": "123 Main St", ... }

# 3. Launch browser (MCP tool)
launch_browser({ url: "https://www.namecheap.com/checkout" })

# 4. Fill non-payment fields with snapshot + fill tools
snapshot()  # see the form
fill({ ref: "@e5", value: "agent@clawcard.sh" })

# 5. Fill payment securely (MCP tool)
fill_checkout({
  cardId: "card_abc",
  email: "agent@clawcard.sh",
  name: "Agent-001",
  addressLine1: "123 Main St",
  addressCity: "San Francisco",
  addressState: "CA",
  addressZip: "94105",
  addressCountry: "US"
})
# → { success: true, filled: ["email", "cardNumber", "stripe:cardExpiry", ...], cardLastFour: "4242" }

# 6. Submit
click({ ref: "@e20" })  # the submit button
```
