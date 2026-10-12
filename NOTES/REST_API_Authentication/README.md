# REST API Authentication: CCNA Study Notes

Organized study note on **authentication schemes used by REST APIs**, part of the CCNA network automation and programmability topics.

## Files

| File | Description |
|---|---|
| `REST_API_Authentication.docx` | The formatted study note (Word document) |
| `README.md` | This file: overview and how the note is organized |

## What the note covers

1. **What Is Authentication?** Definition, what a "resource" is, and why unprotected APIs are a risk.
2. **Authentication Schemes.** Overview of the four schemes.
3. **Basic Authentication.** Username and password sent with every request, Base64-encoded (encoding is not encryption).
4. **Bearer Authentication.** Token sent in the `Authorization` header, with a flow diagram and example header.
5. **API Key Authentication.** A unique key sent in a header to identify the calling application.
6. **OAuth 2.0.** Delegated access with scopes and access tokens, with its four roles and a flow diagram.
7. **Quick Comparison.** Side-by-side table of all four schemes.
8. **Summary.** One-line recap of each scheme.

## Key points to remember

- **Authentication** verifies *who* the client is; resources (data, devices, actions) must be protected from unauthorized requests.
- **Basic**: Base64 is encoding, **not** encryption. Use HTTPS.
- **Bearer**: whoever holds the token gets access, so protect it.
- **API key**: identifies the *application*; keep it secret and prefer headers over URLs.
- **OAuth 2.0**: lets a user grant limited access without sharing a password. It is technically about *authorization*; OpenID Connect handles login on top of it.
- **Always use HTTPS** with every scheme.

## Formatting conventions

- Numbered sections with sub-sections (4.1, 4.2, ...)
- Bullet points for explanations
- Yellow-shaded **KEY RULE** boxes for the most important rules
- Diagrams embedded where they are explained (Bearer flow, OAuth 2.0 flow)
- Gray boxes for example HTTP headers

## Related notes

- REST APIs (SDN and northbound interfaces, CRUD and HTTP verbs, HTTP request vs response)
- Routing Fundamentals
- Static Routes
