# levigo

levigo is a Go wrapper for RocksDB.

The API has been godoc'ed and [is available on the
web](https://github.com/mikefaille/rocksdb-go).

Questions answered at `golang-nuts@googlegroups.com`.

## Building

You'll need the shared library build of
[RocksDB](https://github.com/facebook/rocksdb) installed on your machine. The
current RocksDB will build it by default.

Now, if you build RocksDB and put the shared library and headers in one of the
standard places for your OS, you'll be able to simply run:

    go get github.com/mikefaille/rocksdb-go

But, suppose you put the shared RocksDB library somewhere weird like
/path/to/lib and the headers were installed in /path/to/include. To install
levigo remotely, you'll run:

    CGO_CFLAGS="-I/path/to/rocksdb/include" CGO_LDFLAGS="-L/path/to/rocksdb/lib" go get github.com/mikefaille/rocksdb-go

and there you go.

In order to build with snappy, you'll have to explicitly add "-lsnappy" to the
`CGO_LDFLAGS`. Supposing that both snappy and rocksdb are in weird places,
you'll run something like:

    CGO_CFLAGS="-I/path/to/rocksdb/include -I/path/to/snappy/include"
    CGO_LDFLAGS="-L/path/to/rocksdb/lib -L/path/to/snappy/lib -lsnappy" go get github.com/mikefaille/rocksdb-go

(and make sure the -lsnappy is after the snappy library path!).

Of course, these same rules apply when doing `go build`, as well.

## Caveats

Comparators and WriteBatch iterators must be written in C in your own
library. This seems like a pain in the ass, but remember that you'll have the
RocksDB C API available to your in your client package when you import levigo.

An example of writing your own Comparator can be found in
<https://github.com/mikefaille/rocksdb-go/tree/master/examples>.
