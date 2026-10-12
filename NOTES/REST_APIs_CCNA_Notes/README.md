# REST APIs — CCNA Study Notes

Organized notes on REST APIs for the CCNA network automation and programmability topic.

## Contents

1. **Introduction to APIs** — what an API is and why it matters for network automation
2. **APIs in SDN Architecture** — application vs SDN controller, NBI (REST API) and SBI
3. **CRUD Operations** — Create, Read, Update, Delete mapped to HTTP verbs (POST, GET, PUT, DELETE)
4. **HTTP and REST** — how an application talks to an SDN controller over HTTP
5. **HTTP Verb vs Request vs Response** — the three terms and how they differ
6. **REST API** — definition of REST (section to be completed)

## Key rules at a glance

- Application → REST API → SDN Controller → SBI → Network devices
- The NBI typically uses a REST API; NETCONF and RESTCONF are other APIs used on the southbound side
- Create = POST, Read = GET, Update = PUT, Delete = DELETE
- HTTP verb = the action, HTTP request = the full message, HTTP response = the answer

## Files

| File | Description |
|---|---|
| `REST_APIs_CCNA_Notes.docx` | Formatted notes (numbered sections, key-rule callouts, diagrams) |
| `README.md` | This summary |

## Notes

- The original notes stop at "REST stands for Representational State Transfer", so Section 6 is a placeholder for the rest.
