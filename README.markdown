# General

- All API responses are in JSON format.
- Unless expressly stated otherwise, Google JSON Style Guide is the guiding document for terms, styles and usage.
- Requests for non-JSON responses from API endpoints (e.g. `.json` is excluded from the url and `Accept` is not `application/json`) will be HTTP UNACCEPTABLE unless explicitly supported by the given API
- All integer ids are 64 bit integers.
- POST, PUT and PATCH requests may be submitted in either `application/json` or `application/x-www-form-urlencoded` format
- The `Content-Type` header must be set for POST/PATCH/PUT requests

# HTTP Status Codes

- Status codes should be used as close to their specified meaning as possible
- 200 must always have content
- 201 should always have a `Location` header
- 400 should be used when the client has provided invalid parameters, including parameters which cause the resource to enter an invalid state
- 401 should be used when the client needs to be authenticated to perform the given operation
- 403 should be used when the client is denied access to the action or resource
- 404 should be used when the URI is invalid or a resource could not be located within constraints
- 406 should be used when the `Accept` header cannot be fulfilled (E.G. it is not `application/json`)
- 500 should be used when the server has encountered an error
- 502 and 503 indicate the API is down


# Security

- API authentication is done with JWT using the HTTP Authorization header as a Bearer token.

# Structure



## URL Structure

## JSON Request Structure

- POST / PATCH / PUT requests will follow a nested JSON structure


## JSON Response Structure

- All objects, both primary and nonprimary, should be in collections (e.g. ‘users’: [...] ) in a compound document with the exception of non-model/resource responses such as `meta` and `error`
- All object keys are in camelCase
- Meta will include the resource type (e.g. meta.primaryResourceCollection: ”users”) and primary resource ID (e.g. meta.primaryResourceId: 1)
- Every object reachable via URL may include a URI to access the object

## Associations

- Always send down either all or no associated records. Non-primary collections will not be paginated
- When sending no associated records, optionally include a virtual collection (e.g. Post.top_comments)
- Always include a collection count (e.g. Post.comments_count)
- For each resource, make an explicit decision on each association whether to return all associated records or none by default.
- The API may provide a method to allow clients to request inclusion or exclusion of associations

Examples:
    `User.avatars` is expected to be small. It would return all records.
    `Post.comments` could be potentially large. It would return no records.

# Hypermedia

# Date and Time

- All timestamps are sent and returned in ISO 8601 format (UTC time zone): YYYY-MM-DDTHH:MM:SSZ

# Pagination

- Every paginated collection should contain the URI for the next result set
- Pagination Metadata must contain the current page number and per page record count
- Pagination Metadata may contain total counts
- All links should be considered opaque, meaning they are absolute and contain all parameters necessary to retrieve the intended results (e.g. including page number, size, etc. on a pagination “next” link).

# Attachments

- Always represent attachments as a separate model, specific to the type. This allows for attachments to be consistently represented.
- Include height and width in json and in the URL for images (e.g. {“large”: { url: “foot.co?height=100&width=100”, “height”: 100, “width”: 100} })

Example Models:
 - Images
 - Videos
 - Static Files


Images



# Errors

- Return the proper HTTP status code in the header
- error hash containing:
  - http status code
  - error message intended to be consumed by the end user
  - validation errors (optional) (enumerating through rejected fields)
  - error ID - uniquely identify the error thrown, which can be used to track down in error logs
  - error code (TBD)
  - error code URL to API documentation (TBD; as seen in HAL)
