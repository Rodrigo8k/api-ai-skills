# Base URLs

All BingX OpenAPI endpoints share the same base URL prefix. Choose the environment that matches your use case.

## Environment Table

| Environment ID | Description | Base URL (Primary) | Base URL (Fallback) |
|----------------|-------------|---------------------|---------------------|
| `prod-live` | Production Live | `https://open-api.bingx.com` | `https://open-api.bingx.pro` |
| `prod-vst` | Production Simulated (Testnet) | `https://open-api-vst.bingx.com` | `https://open-api-vst.bingx.pro` |

> **Default:** Use `prod-live` for real trading. Use `prod-vst` for paper trading / testing without real funds.
>
> **Fallback:** When the primary domain is unreachable (network error), automatically retry with the fallback domain.

## TypeScript Helper

Use this constant in your code to switch environments easily. Each environment maps to an array of URLs — primary first, fallback second:

```typescript
const BASE_URLS: Record<string, string[]> = {
  "prod-live": ["https://open-api.bingx.com", "https://open-api.bingx.pro"],
  "prod-vst":  ["https://open-api-vst.bingx.com", "https://open-api-vst.bingx.pro"],
};

function getBaseUrls(env: string = "prod-live"): string[] {
  return BASE_URLS[env] ?? BASE_URLS["prod-live"];
}
```
