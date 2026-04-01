# Paying on Checkout Pages

**FOLLOW THESE STEPS IN ORDER:**

1. **Check existing cards FIRST:**
   ```
   clawcard agent cards --json
   ```
   Look for an open card matching this merchant. If found, skip to step 4.

2. **Check budget:**
   ```
   clawcard agent budget --json
   ```

3. **Create a card ONLY if no existing card works:**
   ```
   clawcard agent cards create --amount <cents> --type single_use --memo "description" --json
   ```

4. **Get billing address:**
   ```
   clawcard agent billing-address --json
   ```

5. **Fill non-payment fields** (name, email, address, zip) using the billing address.

6. **Get card details:**
   ```
   clawcard agent cards details <card-id> --json
   ```

7. **Fill card fields** (card number, expiry, CVC) yourself if possible.

8. **If card fields are in iframes**, use the browser extension:
   ```
   clawcard agent pay --card-id <card-id> --json
   ```

9. **Verify fields are filled**, then submit the payment.
