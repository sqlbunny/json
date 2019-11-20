# json

This is a fork of Go's standard `encoding/json`, with extra focus on parsing strictness.

List of new features:

- `CaseSensitiveFields`: matches struct fields case-sensitively
- `DisallowDuplicateFields`: Fails when unmashaling an object into a struct, and the object contains the same struct field multiple times.
- `DisallowNullPrimitives`: Does not allow unmarshaling `null`s into non-nullable types, essentially primitives (ints, strings..) and non-pointer structs.
- `CheckArraySizes`: When unmarshaling into a Go fixed size array (not a slice), checks that the JSON array is the same size as the Go array. (The stdlib behavior is to truncate if the JSON array is longer, and to fill with zero values if the JSON array is shorter.)
- `json:"foo,required"` struct tag, to force a field to be present when unmarshaling.

## Usage

- Replace your usages of `encoding/json` with `github.com/sqlbunny/json`.
- Activate your desired extra strictness by calling methods such as `DisallowDuplicateFields` on your Decoder instances.
- Done!

`github.com/sqlbunny/json` is a drop-in replacement for `encoding/json`. This means, if you just replace your usages it will behave in exactly the same way. The extra strictness only happens if you explicitly enable it.

## Motivation

This fork of `json` is designed for unmarshaling request bodies of a JSON REST API. If
your API is too liberal when unmarshaling JSON, clients might end up depending
on that behavior, forcing you to support it forever. The best solution is to be
as strict as possible.

## Update procedure

The goal of this repository is to track upstream `encoding/json` as closely as possible. 

To update, the following is done:

    git remote add upstream git@github.com:golang/go
    git fetch upstream
    git checkout -b upstream
    git reset --hard upstream/master
    git filter-branch -f --prune-empty --subdirectory-filter src/encoding/json/ HEAD
    git checkout master
    git rebase upstream
