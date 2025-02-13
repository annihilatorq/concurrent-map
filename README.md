Slightly modified version of concurrent-map, the interface of main functions is adjusted to sync.Map (for consistency with the standard), and also added functionality for hashing integer-type keys (in the original version each integer should be converted to a string, and then hashed with fnv1). in this version fnv1 is replaced by fnv1a, and hashing of integer types is performed with splitmix64.

---
As explained [here](http://golang.org/doc/faq#atomic_maps) and [here](http://blog.golang.org/go-maps-in-action), the `map` type in Go doesn't support concurrent reads and writes. `concurrent-map` provides a high-performance solution to this by sharding the map with minimal time spent waiting for locks.

Prior to Go 1.9, there was no concurrent map implementation in the stdlib. In Go 1.9, `sync.Map` was introduced. The new `sync.Map` has a few key differences from this map. The stdlib `sync.Map` is designed for append-only scenarios. So if you want to use the map for something more like in-memory db, you might benefit from using our version. You can read more about it in the golang repo, for example [here](https://github.com/golang/go/issues/21035) and [here](https://stackoverflow.com/questions/11063473/map-with-concurrent-access)

## usage

Import the package:

```go
import (
	cmap "github.com/annihilatorq/concurrent-map"
)

```

```bash
go get github.com/annihilatorq/concurrent-map@v1.1.0
```

The package is now imported under the "cmap" namespace.

## example

```go

	// Create a new map.
	m := cmap.New[string]()

	// Sets item within map, sets "bar" under key "foo"
	m.Set("foo", "bar")

	// Retrieve item from map.
	bar, ok := m.Get("foo")

	// Removes item under key "foo"
	m.Remove("foo")

```

For more examples have a look at concurrent_map_test.go.

Running tests:

```bash
go test "github.com/annihilatorq/concurrent-map"
```

## license
MIT (see [LICENSE](https://github.com/orcaman/concurrent-map/blob/master/LICENSE) file)
