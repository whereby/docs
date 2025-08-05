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

We rate limit our APIs, returning a `429` status code response when your request has been throttled. You will have to wait and retry the request later.

We use a point system to limit the number of requests that you can make.\
A read request consumes 1 point, a delete consumes 2 points and a write request consumes 3 points.\
Here are some examples of how a 1000 point budget could be spent within **1 minute**:

* 1000 GET requests (1000 x 1 = 1000 points)
* 500 DELETE requests (500 x 2 = 1000 points)
* 333 POST requests (333 x 3 = 999 points)
* 200 DELETE + 100 POST + 300 GET requests (200 x 2 + 100 x 3 + 300 x 1 = 1000 points)

_Enterprise_ [plans](https://whereby.com/information/embedded/pricing/) have 1000 points in total per minute, and _Build_ plans have 100. The limits are global and are shared across all your API keys. Talk to us if you need higher limits.

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
