# openrouter_api

An asynchronous Rust client library for the OpenRouter API. The crate exposes typed request and response models, a builder-style HTTP client, API modules, and an HTTP MCP client. It does not implement the Python task router, local SQLite cost logger, or automatic Claude Code routing described by the previous README.

## Status and provenance

| Item | Repository evidence |
|---|---|
| Crate | `openrouter_api` |
| Version in `Cargo.toml` | `0.5.1` |
| Rust edition | 2021 |
| Toolchain | Stable Rust, per `rust-toolchain.toml` |
| Runtime and HTTP | Tokio and reqwest |
| License | MIT OR Apache-2.0, per Cargo metadata |
| Cargo author and upstream URL | James Ray; `socrates8300/openrouter_api`, per `Cargo.toml` |
| Current checkout | `hmzainjamil/openrouter_api`; relationship to the Cargo metadata upstream is not established by this README |

Version and dependency details describe the checked-in manifest and may differ from a published package. Check the manifest and release source before relying on them.

## What is included

- Chat completions, legacy completions, embeddings, credits, generation metadata, key information, model and provider APIs, and web search modules.
- Typed data models and request configuration.
- An HTTP client with configurable endpoint, timeout, retries, and response-size controls.
- An MCP client for communicating with a configured MCP server.
- Structured-output support and examples.
- Unit, integration, compile-fail, and type-safety test sources.

The crate sends requests to OpenRouter when you call its API methods. The MCP client sends HTTP requests to the server URL you configure. Review the endpoints and data you pass before use. This library does not provide a local model or guarantee provider routing, price, availability, or output quality.

## Use from a Rust project

Add the crate from a registry only after confirming the package and version you intend to use. To work from this checkout:

```toml
[dependencies]
openrouter_api = { path = "../openrouter_api" }
tokio = { version = "1", features = ["macros", "rt-multi-thread"] }
```

Set an API key in your shell. The client helper reads `OPENROUTER_API_KEY`, then `OR_API_KEY`.

```sh
export OPENROUTER_API_KEY='your-key'
```

Basic asynchronous request, following the crate's typed models:

```rust
use openrouter_api::types::chat::{ChatRole, Message};
use openrouter_api::{OpenRouterClient, Result};

#[tokio::main]
async fn main() -> Result<()> {
    let client = OpenRouterClient::from_env()?;
    let messages = vec![Message::text(ChatRole::User, "Say hello.")];

    // API method names and request types are defined by this checkout.
    let response = client.chat().completion(
        "openai/gpt-4o",
        messages,
    ).await?;

    println!("{response:?}");
    Ok(())
}
```

Check the current client and chat module signatures before copying an example into an application. For a fuller structured-output example, see [examples/structured_output.rs](./examples/structured_output.rs). It makes a real provider request when run and needs a valid key, network access, and a supported model.

## MCP client

The crate also exports `MCPClient`. The checked-in [MCP example](./examples/mcp_client.rs) uses a placeholder URL and requires a reachable MCP server that implements the expected HTTP JSON-RPC methods. Replace the placeholder only with a server you trust. Calls can disclose request data to that server and may invoke its tools.

## Configuration and security

- Keep API keys in environment variables or a secret manager. Do not commit them.
- The client defaults to OpenRouter's API URL, a 30 second timeout, and a 10 MiB maximum response size in the current source. Review `ClientConfig` before changing limits.
- The manifest enables rustls TLS by default and offers a native TLS feature. Its source enforces that the two TLS feature selections cannot be enabled together.
- The `allow-http` feature exists in the manifest. Avoid sending credentials or sensitive data over unencrypted HTTP.
- Review [SECURITY_ADVISORY.md](./SECURITY_ADVISORY.md) for the repository's historical security notes. Confirm affected versions and current fixes against the source and release you use.
- The crate cannot decide whether an endpoint, model, MCP server, or data-sharing choice is appropriate for your application.

## Repository guide

| Path | Purpose |
|---|---|
| [src/lib.rs](./src/lib.rs) | Public module exports and TLS feature guard |
| [src/client.rs](./src/client.rs) | Main client and builder states |
| [src/api/](./src/api/) | API request operations |
| [src/types/](./src/types/) | Request and response types |
| [src/mcp/](./src/mcp/) | MCP client and protocol types |
| [examples/](./examples/) | Rust usage examples |
| [tests/type_safety/README.md](./tests/type_safety/README.md) | Compile-fail test guide |
| [CONTRIBUTING.md](./CONTRIBUTING.md) | Contribution workflow |
| [CHANGELOG.md](./CHANGELOG.md) | Recorded changes |
| [LICENSE](./LICENSE) | License text |

The `docs/` directory contains supporting SQL and audit artifacts. It is not a generated API documentation site. Rust API documentation is generated from source comments with Cargo.

## Build and verification

Install the stable Rust toolchain, then run these commands from the repository root:

```sh
cargo check
cargo test
cargo doc --no-deps
```

The repository also includes `scripts/pre_quality.sh`; inspect that script before running it because it may invoke additional checks or tools. Tests and CI files in the repository describe available checks, not proof that this checkout passes them. No build or tests were run for this README update.

## Contributing and license

See [CONTRIBUTING.md](./CONTRIBUTING.md). The manifest declares dual MIT or Apache-2.0 licensing; consult [LICENSE](./LICENSE) and preserve existing author and upstream attribution when redistributing.
