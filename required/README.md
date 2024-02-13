# All Root CAs required for BTP components and consumers
This is the list of Root CAs that each component running in BTP and all consumers of BTP must trust.

All Root CAs are approved by SAP.

See certificate BOM in [required.md](required.md).

## Typical use cases:

1. Your client shall be able to connect with BTP services.

2. You run a custom domain with mutual TLS that shall allow connects from BTP clients.

3. You run a custom domain with mutual TLS that shall allow connects from non-BTP clients, which got a client certificate from a BTP certificate service.
