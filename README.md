---
title: "sb-stack-demo-docs"
description: "Quick reference guide with code examples for sb-stack services and clients"
nav_order: 0
---

# sb-stack-demo-docs
> TODO
## Documentation Index

### Transports

- **[1.0 — Transport Layer Architecture](1.0-transports.md)**  
  Generic abstraction powering all transports

- **[1.1 — HTTP Transport](1.1-http-transport.md)**  
  HTTP/1.1 and HTTP/2 with connection pooling and middleware

- **[1.2 — WebSocket Transport](1.2-ws-transport.md)**  
  Real-time bidirectional communication with pub/sub patterns

### Protocols

- **[2.0 — Protocol Services](2.0-protocols.md)**  
  Overview of protocol-specific services

- **[2.1 — JSON-RPC Service](2.1-jsonrpc.md)**  
  JSON-RPC 2.0 implementation with HTTP and WebSocket support

- **[2.2 — gRPC Service](2.2-grpc.md)**  
  gRPC protocol implementation *(work in progress)*

### Clients

- **[4.0 — Building Blockchain RPC Clients](4.0-chains.md)**  
  Guide to assembling transports and services into blockchain clients

- **[4.1 — Ethereum Client](4.1-eth.md)**  
  Complete Ethereum JSON-RPC client with batch processing and subscriptions

## Quick Examples

### HTTP Transport

**Rust**
```rust
use sb_stack_transport_http::{HttpTransport, Http1TransportConfig, HttpTransportRequestPacket};

async fn example() -> Result<(), Box<dyn std::error::Error>> {
    let base_url = "https://httpbin.org".parse()?;
    let mut transport = HttpTransport::new(base_url.clone(), Http1TransportConfig::default(), None);
    transport.connect().await?;
    let mut request = HttpTransportRequestPacket::new(http::Method::GET);
    request.set_url(Some(base_url.join("/get")?));
    let response = transport.call(request).await?;
    Ok(())
}
```

**Python**
```python
from sb_http_client import HttpClient

async def example():
    client = HttpClient("https://httpbin.org")
    response = await client.get("/get")
    print(f"Status: {response.status}")
```

**Typescript**
```typescript
import { HttpClient } from 'sb-http-client';

async function example() {
    const client = new HttpClient({ baseURL: 'https://httpbin.org' });
    const response = await client.get('/get');
    console.log('Status:', response.status);
}
```

### Eth Client

**Rust**
```rust
use sb_eth_jsonrpc_client::EthJsonrpcClient;

async fn example() -> Result<(), Box<dyn std::error::Error>> {
    let client = EthJsonrpcClient::new("https://eth-rpc-url-goes-here.com");
    client.connect().await?;
    let block = client.eth().get_block_by_number(12345.into(), true).await?;
    println!("Block: {}", block.number);
    client.close().await?;
    Ok(())
}
```

**Python**
```python
from sb_eth_jsonrpc_client import EthJsonrpcClient

async def example():
    client = EthJsonrpcClient("https://eth-rpc-url-goes-here.com")
    await client.connect()
    block = await client.eth().get_block_by_number(12345, True)
    print(f"Block: {block.number}")
    await client.close()
```

**Typescript**
```typescript
import { EthJsonrpcClient } from 'sb-eth-jsonrpc-client';

async function example() {
    const client = new EthJsonrpcClient("https://eth-rpc-url-goes-here.com");
    await client.connect();
    const block = await client.eth().getBlockByNumber(12345, true);
    console.log(`Block: ${block.number}`);
    await client.close();
}
```
