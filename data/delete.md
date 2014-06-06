# 删除一个文档

The syntax for deleting a document follows the same pattern that we have seen
already, but uses the `DELETE` method :

```js
DELETE /website/blog/123
```


If the document is found, Elasticsearch will return an HTTP response code
of `200 OK` and a response body like the following. Note that the `_version`
number has been incremented.

```js
{
  "found" :    true,
  "_index" :   "website",
  "_type" :    "blog",
  "_id" :      "123",
  "_version" : 3
}
```

If the document isn't found, we get a `404 Not Found` response code, and
a body like:

```js
{
  "found" :    false,
  "_index" :   "website",
  "_type" :    "blog",
  "_id" :      "123",
  "_version" : 4
}
```

Even though the document doesn't exist -- `"found"` is `false` --  the
`_version` number has still been incremented. This is part of the internal
book-keeping which ensures that changes are applied in the correct order
across multiple nodes.

****

As already mentioned in <<update-doc>>, deleting a document doesn't
immediately remove the document from disk -- it just marks it as deleted.
Elasticsearch will clean up deleted documents in the background as you
continue to index more data.

****

