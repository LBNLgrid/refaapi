# API Access

## Base URL

```
https://refa.lbl.gov
```

## Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/refa-api/v1/calculate-feasible-projects` | Calculate feasible transmission line upgrade projects and their costs (main endpoint, see [Payload Structure](payload.md)) |

## Authentication (OAuth2 client credentials)

All requests must carry a bearer access token. Tokens are obtained from the OAuth token endpoint using HTTP Basic client authentication:

- **Token URL**: `https://refa.lbl.gov/oauth/token`
- **Client credentials** (`CLIENT_ID` / `CLIENT_SECRET`): get them from the REFA API admin panel, under **"API Keys"**.
- **Grant type**: `client_credentials`
- **Scope**: `refa:calculate`

Token request (the `Authorization` header is `Basic base64(CLIENT_ID:CLIENT_SECRET)`):

```bash
curl -X POST "https://refa.lbl.gov/oauth/token" \
  -u "$CLIENT_ID:$CLIENT_SECRET" \
  -d "grant_type=client_credentials" \
  -d "scope=refa:calculate"
```

Response:

```json
{
  "access_token": "eyJhbGciOi...",
  "token_type": "Bearer",
  "expires_in": 3600,
  "scope": "refa:calculate"
}
```

## Querying the calculate-feasible-projects endpoint

Send the payload as a JSON body with the bearer token:

```bash
curl -X POST "https://refa.lbl.gov/refa-api/v1/calculate-feasible-projects" \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d @payload_calculate-feasible-projects.json
```

Equivalent Python for a ready-to-run script:

```python
import base64, json
from urllib.parse import urlencode
from urllib.request import Request, urlopen

CLIENT_ID = "..."       # from the REFA API admin panel, under "API Keys"
CLIENT_SECRET = "..."   # from the REFA API admin panel, under "API Keys"
TOKEN_URL = "https://refa.lbl.gov/oauth/token"
API_URL = "https://refa.lbl.gov/refa-api/v1/calculate-feasible-projects"

# 1. Get the access token
credentials = base64.b64encode(f"{CLIENT_ID}:{CLIENT_SECRET}".encode()).decode("ascii")
token_request = Request(
    TOKEN_URL,
    data=urlencode({"grant_type": "client_credentials", "scope": "refa:calculate"}).encode(),
    headers={"Authorization": f"Basic {credentials}",
             "Content-Type": "application/x-www-form-urlencoded"},
    method="POST",
)
with urlopen(token_request, timeout=60) as response:
    access_token = json.load(response)["access_token"]

# 2. Send the payload
with open("payload_calculate-feasible-projects.json", encoding="utf-8") as f:
    payload = json.load(f)
api_request = Request(
    API_URL,
    data=json.dumps(payload).encode(),
    headers={"Authorization": f"Bearer {access_token}",
             "Content-Type": "application/json"},
    method="POST",
)
with urlopen(api_request, timeout=120) as response:
    results = json.load(response)
```


