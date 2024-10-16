# Protobufs in Onyx

[Protobuf](https://protobuf.dev) support in [Onyx](https://onyxlang.dev).
This package only provides functional for marshaling and unmarshaling data from protobufs.
To generate Onyx bindings from `.proto` definitions, use [protoc-gen-onyx](https://github.com/onyx-lang/protoc-gen-onyx).

## Usage

Given `api.proto` containing

```protobuf
package my_api;

message Query {
    string name = 1;
    optional int32 max_results = 2;
}
```

You need to generate Onyx bindings using [protoc-gen-onyx](https://github.com/onyx-lang/protoc-gen-onyx).

Use this command,
```shell
$ protoc --plugin=protoc-gen-onyx --onyx_out=protos api.proto
```

To generate `protos/api.proto.onyx`.
```odin
package protobuf.my_api

use protobuf

@protobuf.Message
Query :: struct {
    @protobuf.Field.{1, 9}
    name: str
    @protobuf.Field.{2, 5}
    max_results: ? i32
}
```

You can the `protobuf` package to marshal/unmarshal the data.

```odin
use protobuf
use protobuf.my_api { Query }

main :: () {
    query := Query.{
        name = "foo"
        max_results = 10
    }

    // Marshal data
    query_data := protobuf.marshal(query)!  // This doesn't do any error handling

    // Unmarshal data
    decoded_query := protobuf.unmarshal(query_data, Query)!

    logf(.Info, "Query: {p}", decoded_query)
}
```

## Missing features

- `oneof` handling
- `LEN` field concatenation



