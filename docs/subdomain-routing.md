# Subdomain Folder Routing

Orator's static file serving includes a built-in feature for subdomain-based folder routing. This enables a simple multi-tenant static hosting setup without any additional configuration or infrastructure.

## How It Works

When a request arrives, Orator inspects the `Host` header for subdomain prefixes. If the host has more than one segment (e.g., `clienta.example.com` vs just `localhost`), Orator takes the first segment and checks if a matching subfolder exists in the serve directory.

<!-- bespoke diagram: edit diagrams/how-it-works.mmd or .hints.json, then: npx pict-renderer-graph build modules/orator/orator-static-server/docs -->
![How It Works](diagrams/how-it-works.svg)

## Setup

```javascript
// Serve from ./sites, with subdomain-based subfolder routing
_Fable.Orator.addStaticRoute('./sites/');
```

## Directory Structure

<!-- bespoke diagram: edit diagrams/directory-structure.mmd or .hints.json, then: npx pict-renderer-graph build modules/orator/orator-static-server/docs -->
![Directory Structure](diagrams/directory-structure.svg)

## Example Requests

| Request URL | Served File |
|------------|-------------|
| `http://example.com/index.html` | `./sites/index.html` |
| `http://clienta.example.com/index.html` | `./sites/clienta/index.html` |
| `http://clientb.example.com/styles.css` | `./sites/clientb/styles.css` |
| `http://unknown.example.com/page.html` | `./sites/page.html` (no match, falls back) |

## Limitations

- Subdomain routing is one level deep -- it only checks the first subdomain segment
- The subfolder must exist on disk for the routing to activate
- Requests to `localhost` or IP addresses won't trigger subdomain routing (only one host segment)

## Use Cases

- **Multi-tenant SaaS frontends** - Each customer gets a branded subdomain serving their customized UI
- **Development environments** - Different feature branches served from subdomains
- **A/B testing** - Serve different static builds based on subdomain
