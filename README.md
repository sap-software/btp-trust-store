# Trust Store for SAP BTP
The BTP Trust Store is a list of trusted X.509 certificates of Root Certificate Authorities that are used by BTP, and which shall be used and regularly updated by customers in their BTP clients to ensure disruption-free TLS communication.

Two folders are provided, **required** and **optional**.

## The `REQUIRED` BTP Trust Store

This is the list of Root CAs that each component running in BTP and all consumers of BTP **MUST** trust.

Customers **SHOULD** use this trust store in their own applications to make sure they are compatible with current and future PKIs used by BTP.

All Root CAs are approved by SAP.

### Typical use cases:

1. Your client connects with BTP services.

2. You run a custom domain with mutual TLS that allows connections from BTP clients.

3. You run a custom domain with mutual TLS that allows connections from non-BTP clients, which got a client certificate from a BTP certificate service.


## The `OPTIONAL` BTP Trust Store

This is the complete list of Root CAs approved by SAP.

All `REQUIRED` Root CAs are contained.

### Typical use cases:

1. Your client connects with BTP and non-BTP services, and the assumption is that the target server certificate was issued by one of these PKIs.

2. You run a custom domain with mutual TLS that allows connections from BTP and non-BTP clients, and the assumption is that the client certificate was issued by one of these PKIs.


## Version of the BTP Trust Store

The repository comes with GitHub Releases and a SEMVER compliant version scheme:

- Major version increase: CA(s) where removed from one of the stores
- Minor version increase: CA(s) where added to one of the stores
- Patch version increase: Correction of minor version CA change
- No new Release and version for other changes like READMEs

**Example:**

- `v1.0.0` - Initial release
- `v1.1.0` - New CA added to optional
- `v1.1.1` - Correction of CA added to optional
- `v2.0.0` - Disapproved CA removed from required
- `v2.1.0` - New CA added to required

## Implementation

The BTP Trust Store will be changed whenever a new PKI is introduced in BTP, or an old PKI that was used in the past is not active anymore.

New PKIs are added with sufficient lead time, before they are actively used by BTP. It is important to consume the BTP Trust Store regularly, at least once per quarter, to make sure certificates issued by such new PKI CA can be validated successfully.

Old PKIs are removed after sufficient follow-up time, i.e. when the all issued certificates used in BTP are expired, and the PKI CAs are not actively used by BTP anymore.

### Consuming the stores

All PEM files are plain text files, they do not contain private keys. You may have to concatenate all single `.pem` files of the desired store flavour if your target environment is expecting a single CA file (`.pem` or `.crt`) with multiple entries.

The store file type PSE (for ABAP or HANA, resp. SAP CommonCryptoLib) is not password protected, they do not contain private keys.

The store file types JKS (for Java) and P12 (for Java >= 9) are password protected by design, although they do not contain private keys.

> The key store password for all JKS and P12 files is `changeit`.
