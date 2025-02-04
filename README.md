# tsync

<a href="https://crates.io/crates/tsync"><img src="https://img.shields.io/crates/v/tsync.svg?style=for-the-badge" height="20" alt="License: MIT OR Apache-2.0" /></a>

A utility to generate typescript types from rust code.

# Install

There are two parts to this:

1. The CLI tool:

   ```
   cargo install tsync
   ```

2. The dependency for rust projects (to use the `#[tsync]` attribute; see usage below)

   ```
   /// Cargo.toml

   tsync = "X.Y.Z"
   ```

# Usage

Mark structs with `#[tsync]` as below:

```rust
/// src/main.rs
use tsync::tsync;

/// Doc comments are preserved too!
#[tsync]
struct Book {
  name: String,
  chapters: Vec<Chapter>,
  user_reviews: Option<Vec<String>>
}

#[tsync]
struct Chapter {
  title: String,
  pages: u32
}


#[tsync]
/// Time in UTC seconds
type UTC = usize;
```

Then use the CLI tool:

```sh
tsync -i ./src -o types.d.ts
```

And voilà!

```ts
/// types.d.ts

/* This file is generated and managed by tsync */

// Doc comments are preserved too!
interface Book {
  name: string
  chapters: Array<Chapter>
  user_reviews: Array<string> | undefined
}

interface Chapter {
  title: string
  pages: number
}

// Time in UTC seconds
type UTC = number
```

## Multiple Inputs

You can specify many inputs (directories and/or files) using the `-i` flag multiple times, like so:

```sh
tsync -i directory1 -i directory2 -o types.d.ts
```

## Multiple Outputs

It might help to create multiple typing files for your project. It's easy, just call tsync multiple times:

```sh
tsync -i src/models -o models.d.ts
tsync -i src/api -o api.d.ts
```

# Errors

A list of files which can't be opened or parsed successfully are listed after executing `tsync`. For other errors, try using the `--debug` flag to pinpoint issues. Please use the Github issue tracker to report any issues.

# Docs

See `tsync --help` for more information.

Feel free to open tickets for support or feature requests.

# Development/Testing

Use `./test/test_all.sh` to run tests.
After running the test, there should be no unexpected changes to files in `./test` (use `git status` and `git diff` to see if there were any changes).

# License

This tool is distributed under the terms of both the MIT license and the Apache License (Version 2.0).

See LICENSE-APACHE, LICENSE-MIT, and COPYRIGHT for details.
