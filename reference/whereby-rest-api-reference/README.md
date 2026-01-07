# REST API Reference

### Versioning

As our API continues to evolve it will be versioned to ensure backward compatibility for our users. Breaking changes will come under new major versions, while non-breaking changes may be added to existing versions and change more frequently.

#### Breaking changes:

* Removal of endpoints
* Removing response data
* Changes in request / response format
* Changes to authentication mechanism

#### Non-breaking changes:

* Adding new endpoints
* Adding to response data
* New parameters

### Rate Limiting (throttling)

Our API uses a **point-based rate limiting system** to fairly allocate resources across different types of requests.

Each API request consumes between **1 and 10 points**, depending on its complexity. Point usage is based on factors such as the amount of data processed, the number of resources involved, and whether additional computation or advanced features are required.

_Enterprise_ [plans](https://whereby.com/information/embedded/pricing/) have 1000 points in total per minute, and _Build_ plans have 100. The limits are global and are shared across all your API keys. Talk to us if you need higher limits. Once this limit is exceeded, further requests will be rejected with a `429 Too Many Requests` response until the limit resets.

#### Typical point usage

Point usage depends on **request complexity**, not just whether a request is a read or a write.

* **Low-complexity requests** (for example, simple reads of a single resource): _1 point_
* **Moderate-complexity requests** (such as writes or reads involving multiple resources): _3 points_
* **High-complexity requests** (such as complex reads, aggregations, or batch operations): _5–10 points_

Read-only requests are not guaranteed to be low-cost; some heavy read operations may consume the maximum number of points depending on the amount of data processed and computation required.

#### Rate limit enforcement

When a rate limit is exceeded, the API returns a `429 Too Many Requests` response. Requests will continue to be rejected until sufficient points are available again.

To protect platform stability and ensure fair usage, we may temporarily adjust rate limits or point budgets in response to abusive behavior or unexpected traffic patterns. If your application consistently reaches its rate limit, please contact us to discuss higher limits or alternative approaches.

You may read the `Retry-After` (seconds) or `X-RateLimit-Reset` (date) response headers in order to know when you can restart your requests. In addition, the `X-RateLimit-Limit` header includes the total number of points for your organization, and the `X-RateLimit-Remaining` header displays the remaining available points.

## Authentication

### BearerAuth

The bearer token will be provided upon request and it is up to the client to keep it secret. Every API call needs to contain this token in order to authenticate and authorize the client.

| **Security Scheme Type**      | HTTP   |
| ----------------------------- | ------ |
| **HTTP Authorization Scheme** | bearer |

### Endpoints

* [/meetings](meetings.md)
* [/insights](insights.md)
* [/recordings](recordings.md)
* [/transcriptions](transcriptions.md)
* [/summaries](summaries.md)
* [/rooms](rooms.md)
