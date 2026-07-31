---
name: Triage Newforma Konekt project issues
description: Query, search, and inspect BIM-coordination issues in a Newforma Konekt project, including archived issues, using the Konekt REST API v3.
api: openapi/newforma-konekt-openapi-original.json
operations:
- GetIssues_V3
- SearchIssues_V3
- GetIssue_V3
- GetIssuesDetails_V3
- GetArchivedIssues_V3
- GetMultiple
---

# Triage Newforma Konekt project issues

Operating instructions for an agent working with issues in a Newforma Konekt (formerly BIM Track) project.

## Setup
- Base URL: `https://api.bimtrackapp.co` (version is optional and defaults to `/v3`).
- Authenticate with an OAuth 2.0 bearer access token (PKCE authorization-code or client-credentials from `https://auth.bimtrackapp.co`) or a Hub-owner API access token. Send it as `Authorization: Bearer <token>`. Scope `BIMTrack_Api` is required. See `authentication/newforma-authentication.yml` and `scopes/newforma-scopes.yml`.
- You need the target `hubId` and `projectId`. Resolve the hub first with `GET /v3/hubs` or `GET /v3/hubs/name/{hubName}`.

## Steps
1. **List issues** — call `GetIssues_V3` (`GET /v3/hubs/{hubId}/projects/{projectId}/issues`) to enumerate active issues in the project.
2. **Search** — narrow with `SearchIssues_V3` (`GET .../issues/search`) when you have search criteria rather than a full list.
3. **Bulk fetch** — for many known issues, use `GetMultiple` (`GET .../issues/multiple`) or `GetIssuesDetails_V3` (`GET .../issues/details`) to pull detailed records in one call rather than looping single gets.
4. **Inspect one** — call `GetIssue_V3` (`GET .../issues/{issueId}`) for a single issue's full record.
5. **Check archive** — call `GetArchivedIssues_V3` (`GET .../archivedissues`) when an issue is not among the active set; it may have been archived.

## Conventions & error handling
- Responses are JSON (OData content negotiation available; default `application/json` is fine). See `conventions/newforma-conventions.yml`.
- Errors follow RFC 7807 ProblemDetails. Handle `401` (refresh/reissue token), `403` (caller lacks the hub/project role), `404` (bad hub/project/issue id), and `409` (the calling user must complete their Konekt profile first). See `errors/newforma-problem-types.yml`.
- No idempotency-key or documented pagination; collection endpoints return arrays.
