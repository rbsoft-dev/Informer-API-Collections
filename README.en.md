# Informer API Collections

[RU Русский](README.md) | [EN English](README.en.md)

A [Bruno](https://www.usebruno.com) request collection for testing the REST API of
[Informer](https://github.com/rbsoft03/Informer) — a tray application that receives
notifications over HTTP from external systems (1C, POS software, etc.).

## How to open the collection

1. Clone this repository:
   ```powershell
   git clone https://git.rbsoft.ru/ershov/informer-api-collections.git
   ```
2. Open Bruno → **Open Collection** → point it at the `informer-api-collections` folder
3. Bruno will pick up all the requests and show them in the sidebar

## Set up an environment (required before the first run)

Environment files (`environments/`) are **intentionally not stored in this repository** —
they usually end up containing real API key values, which are sensitive data that
shouldn't live in an open Git repository.

Create your own environment locally:

1. In Bruno: **Right-click the collection → Configure → Environments → Create Environment**
2. Name it, for example, `Local`
3. Add the variables:

| Variable | Value (example) |
|---|---|
| `baseUrl` | `http://127.0.0.1:4399` |
| `apiKey` | `<your key from Settings → API keys>` (if "Require API key" is enabled in Informer) |

4. Select this environment from the dropdown at the top before running requests

> ⚠️ Informer's default port is **`4399`** — check the actual port in Informer's Settings
> if it was changed.

## Requests included in the collection

| Request | Method | Endpoint | Purpose |
|---|---|---|---|
| 01-Notify | `POST` | `/api/notify` | Send a test notification (a toast will appear in Informer) |
| 02-GetHistory | `GET` | `/api/history` | Get the notification list with filters/pagination |
| 03-GetSenders | `GET` | `/api/history/senders` | List of unique senders (for the filter) |
| 04-GetSettings | `GET` | `/api/settings` | Current application settings |
| 05-CreateAPIKey | `POST` | `/api/apikeys` | Create a new API key |
| 06-RateLimitCheck | `POST` | `/api/notify` | Check the anti-spam rate limit (several quick requests in a row) |

## `01-Notify` request body format

```json
{
  "header": "TestSender",
  "description": "Test message from Bruno",
  "type": "info",
  "ResponseBody": {
    "any": "arbitrary structure"
  }
}
```

`type` is an optional field: `info` (default), `warning`, or `error` — affects the toast's
border color in Informer. For format details, see the
[main project's README](https://github.com/rbsoft03/Informer#api--incoming-request-format).

## If Informer requires an API key

The `X-Api-Key` header is already set in the collection's requests as `{{apiKey}}` — it
will be filled in automatically from the environment variable you created above. If
"Require API key" is turned off in Informer (Settings → Security), the header can be left
empty without causing an error.

## Related repositories

- [Informer](https://github.com/rbsoft03/Informer)
