# Email Verifier - JavaScript Library

[![npm version](https://img.shields.io/npm/v/email-verifier.svg)](https://www.npmjs.com/package/email-verifier)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![GitHub stars](https://img.shields.io/github/stars/EnrowAPI/email-verifier-js)](https://github.com/EnrowAPI/email-verifier-js)
[![Last commit](https://img.shields.io/github/last-commit/EnrowAPI/email-verifier-js)](https://github.com/EnrowAPI/email-verifier-js/commits)

Verify email addresses for deliverability with deterministic verification that works on catch-all domains. Integrate email validation into your workflow to reduce bounce rates and protect sender reputation.

Powered by [Enrow](https://enrow.io) — deterministic email verification, not probabilistic.

## Installation

```bash
npm install email-verifier
```

Requires Node.js 18+. Zero dependencies.

## Simple Usage

```typescript
import { verifyEmail, getVerificationResult } from 'email-verifier';

const verification = await verifyEmail({
  apiKey: 'your_api_key',
  email: 'tcook@apple.com',
});

const result = await getVerificationResult('your_api_key', verification.id);

console.log(result.email);          // tcook@apple.com
console.log(result.qualification);  // valid
console.log(result.isDeliverable);  // true
```

`verifyEmail` returns a verification ID. The verification runs asynchronously — call `getVerificationResult` to retrieve the result once it's ready. You can also pass a `webhook` URL to get notified automatically.

## What you get back

```json
{
  "id": "ver_abc123",
  "email": "tcook@apple.com",
  "qualification": "valid",
  "isDeliverable": true,
  "checks": {
    "syntaxValid": true,
    "mxRecordsFound": true,
    "smtpConnectable": true,
    "mailboxExists": true,
    "isCatchAll": false,
    "isDisposable": false,
    "isRoleAccount": false
  },
  "metadata": {
    "domain": "apple.com",
    "mxProvider": "Google",
    "verifiedAt": "2024-12-01T12:00:00Z"
  },
  "creditsUsed": 0.25
}
```

## Bulk verification

```typescript
import { verifyEmails, getVerificationResults } from 'email-verifier';

const batch = await verifyEmails({
  apiKey: 'your_api_key',
  verifications: [
    'tcook@apple.com',
    'satya@microsoft.com',
    'jensen@nvidia.com',
  ],
});

// batch.batchId, batch.total, batch.status

const results = await getVerificationResults('your_api_key', batch.batchId);
// results.results — array of VerificationResult
```

Up to 5,000 verifications per batch. Pass a `webhook` URL to get notified when the batch completes.

## Error handling

```typescript
try {
  await verifyEmail({ apiKey: 'bad_key', email: 'test@test.com' });
} catch (error) {
  // error.message contains the API error description
  // Common errors:
  // - "Invalid or missing API key" (401)
  // - "Your credit balance is insufficient." (402)
  // - "Rate limit exceeded" (429)
}
```

## Getting an API key

Register at [app.enrow.io](https://app.enrow.io) to get your API key. You get **50 free credits** (= 200 free verifications) with no credit card required. Each verification costs **0.25 credits**.

Paid plans start at **$17/mo** up to **$497/mo**. See [pricing](https://enrow.io/pricing).

## Documentation

- [Enrow API documentation](https://docs.enrow.io)
- [Full Enrow SDK](https://github.com/EnrowAPI/enrow-js) — includes email finder, phone finder, reverse email lookup, and more

## License

MIT — see [LICENSE](LICENSE) for details.
