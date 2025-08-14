# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Structure

This is webrtc-rs, a pure Rust implementation of the WebRTC stack that rewrites the Pion WebRTC stack. The project uses a Cargo workspace structure with the following key crates:

- `webrtc/` - Main WebRTC API and peer connection implementation
- `ice/` - ICE (Interactive Connectivity Establishment) protocol
- `dtls/` - DTLS (Datagram Transport Layer Security) implementation  
- `data/` - WebRTC DataChannel implementation
- `rtp/` - RTP (Real-time Transport Protocol) implementation
- `rtcp/` - RTCP (RTP Control Protocol) implementation
- `srtp/` - SRTP (Secure Real-time Transport Protocol) implementation
- `sctp/` - SCTP (Stream Control Transmission Protocol) implementation
- `stun/` - STUN (Session Traversal Utilities for NAT) implementation
- `turn/` - TURN (Traversal Using Relays around NAT) implementation
- `sdp/` - SDP (Session Description Protocol) implementation
- `media/` - Media handling utilities
- `util/` - Common utilities and helper functions
- `interceptor/` - Packet interceptors for stats and NACK/TWCC
- `mdns/` - mDNS implementation
- `examples/` - Example applications demonstrating WebRTC usage

## Common Development Commands

### Building
```bash
# Build all workspace members
cargo build

# Build specific crate (faster for development)
cargo build -p webrtc

# Build examples
cd examples
cargo build --example <example-name>
```

### Testing
```bash
# Run all tests (may be slow)
cargo test

# Test specific crate
cargo test -p webrtc

# Test specific example
cd examples
cargo test --example <example-name>
```

### Code Quality
```bash
# Format code
cargo fmt

# Run clippy linter
cargo clippy

# Check code without building
cargo check
```

### Pre-commit Hooks
The project uses pre-commit hooks with:
- `cargo fmt` for formatting
- `cargo check` for compilation checks

Install with: `pre-commit install`

### Devbox Support
The project supports devbox for development environments:
```bash
devbox run test    # Run tests
devbox run build   # Build project  
devbox run fmt     # Format code
devbox shell       # Enter development shell
```

## Architecture Overview

WebRTC.rs follows the standard WebRTC architecture with these key components:

### Core WebRTC API (`webrtc/`)
- `RTCPeerConnection` - Main entry point for WebRTC connections
- `RTCDataChannel` - Bi-directional data channels over SCTP
- `RTCRtpTransceiver` - RTP media transceivers
- `RTCSessionDescription` - SDP offer/answer handling

### Transport Layer
- **ICE** (`ice/`) - NAT traversal and connectivity establishment
- **DTLS** (`dtls/`) - Secure transport layer over UDP
- **SCTP** (`sctp/`) - Message-oriented transport for data channels

### Media Layer  
- **RTP** (`rtp/`) - Real-time media transport with codec support (H.264, VP8/9, Opus, etc.)
- **RTCP** (`rtcp/`) - Control protocol for media statistics and feedback
- **SRTP** (`srtp/`) - Secure RTP with encryption

### Supporting Protocols
- **STUN/TURN** (`stun/`, `turn/`) - NAT traversal servers
- **SDP** (`sdp/`) - Session description and negotiation
- **mDNS** (`mdns/`) - Local network discovery

### Utilities
- **Interceptors** (`interceptor/`) - Packet processing pipeline for stats, NACK, TWCC
- **Media** (`media/`) - Audio/video sample handling, format readers/writers
- **Util** (`util/`) - Networking utilities, buffers, virtual networking for tests

## Key Design Patterns

- Heavy use of async/await with tokio runtime
- Arc/Mutex for thread-safe shared state
- Trait-based abstractions for extensibility  
- Interceptor pattern for packet processing pipeline
- Comprehensive error handling with thiserror

## MSRV and Dependencies

- **Minimum Supported Rust Version:** 1.65.0
- **Runtime:** Tokio for async I/O
- **Crypto:** Ring and RustTLS for cryptographic operations
- **Serialization:** Serde for configuration and stats

## Testing Guidelines

- Unit tests are co-located with source code in `*_test.rs` files
- Integration tests use examples and cross-crate functionality
- Virtual networking (`vnet`) is used for deterministic network testing
- Fuzz testing is available for some crates (e.g., `sctp/fuzz/`)