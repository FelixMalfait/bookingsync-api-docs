# API Reference

1. TOC
{:toc}

## Accessing the API

The BookingSync API can be accessed over HTTPS at
`https://www.bookingsync.com/api/v3/`.

## JSON

All data is sent and received as JSON.

Regardless whether the response returns a single resource or a collection,
the body is expected to be in the following format:

~~~json
{
  "plural_resource_name": [
    {
      "id": "1",
      "attribute_name": "attribute_value"
    }
  ]
}
~~~

## Request Options

### Pagination

Large collections are paginated, meaning the response will include only
a subset of resources and links to other pages.

The links are sent in the `Link` header, e.g.:

~~~
Link: <https://www.bookingsync.com/api/v3/accounts?page=1>; rel="first", <https://www.bookingsync.com/api/v3/accounts?page=2>; rel="next", <https://www.bookingsync.com/api/v3/accounts?page=3>; rel="last"
X-Total-Pages: 3
~~~

### Limiting Fields

For retrieving collection of large resources, it is sometimes useful to limit
the attributes returned by the API. For example, if we wanted to get only the
`id` and `business_name` fields of the Accounts, we could specify that in the
request:

~~~
GET https://www.bookingsync.com/api/v3/accounts?fields=id,business_name
~~~

## HTTPS

Every request to the API must use HTTPS. When the API is accessed over HTTP,
a 302 redirect will be returned with the correct HTTPS url.

## Rate limiting

Each application accessing the API has a limit of 1000 requests per hour.

Rate limiting information will be returned in the headers:

~~~
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1395140400
~~~

`X-RateLimit-Reset` is the UNIX time at which the next limit reset happens.