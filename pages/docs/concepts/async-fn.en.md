---
description: Run a Rust async fn with the tokio runtime.
---

# async fn

You can do a lot of async/multi-threaded work with `AsyncTask` and `ThreadsafeFunction`, but sometimes you may want to use the crates from the Rust async ecosystem directly.

**NAPI-RS** supports the `tokio` runtime by default. If you `await` a tokio `future` in `async fn`, **NAPI-RS** will execute it in the tokio runtime and convert it into a JavaScript `Promise`.

```rust {6} filename="lib.rs"
use futures::prelude::*;
use napi::bindgen_prelude::*;
use tokio::fs;

#[napi]
async fn read_file_async(path: String) -> Result<Buffer> {
  fs::read(path)
    .map(|r| match r {
      Ok(content) => Ok(content.into()),
      Err(e) => Err(Error::new(
        Status::GenericFailure,
        format!("failed to read file, {}", e),
      )),
    })
    .await
}
```

⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️

```ts filename="index.d.ts"
export function readFileAsync(path: string): Promise<Buffer>
```
