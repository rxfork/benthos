---
title: cache
type: output
---

```yaml
cache:
  key: ${!count:items}-${!timestamp_unix_nano}
  max_in_flight: 1
  target: ""
```

Stores messages in a cache. Caches are configured within the
[resources section](/docs/components/caches/about) and can target any of the following
types:

- dynamodb
- file
- memcached
- memory
- redis
- s3

Like follows:

``` yaml
output:
  cache:
    target: foo
    key: ${!json_field:document.id}

resources:
  caches:
    foo:
      memcached:
        addresses:
          - localhost:11211
        ttl: 60
```

In order to create a unique `key` value per item you should use
function interpolations described [here](/docs/configuration/interpolation#functions).
When sending batched messages the interpolations are performed per message part.

This output benefits from sending multiple messages in flight in parallel for
improved performance. You can tune the max number of in flight messages with the
field `max_in_flight`.

