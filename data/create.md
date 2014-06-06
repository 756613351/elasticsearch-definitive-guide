# 新建一个文档

How can we be sure, when we index a document, that we are creating an entirely
new document and not overwriting an existing one?

Remember that the combination of `_index`, `_type` and `_id` uniquely
identifies a document.  So the easiest way to ensure that our document is new
is by letting Elasticsearch autogenerate a new unique `_id`, using the `POST`
version of the index request:

```js
POST /website/blog/
{ ... }
```

However, if we already have an `_id` that we want to use, then we have to tell
Elasticsearch that it should only accept our index request if a document with
the same `_index`, `_type` and `_id` doesn't exist already. There are two ways
of doing this, both of which amount to the same thing. Use whichever method is
more convenient for you.

The first method uses the `op_type` query string parameter:

```js
PUT /website/blog/123?op_type=create
{ ... }
```

And the second uses the `/_create` endpoint in the URL:

```js
PUT /website/blog/123/_create
{ ... }
```

If the request succeeds in creating a new document, then Elasticsearch will
return the usual metadata and an HTTP response code of `201 Created`.

On the other hand, if a document with the same `_index`, `_type` and `_id`
already exists, Elasticsearch will respond with a `409 Conflict` response
code, and an error message like the following:

```js
{
  "error" : "DocumentAlreadyExistsException[[website][4] [blog][123]:
             document already exists]",
  "status" : 409
}
```

