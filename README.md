# Trust Store for SAP BTP
SAP BTP Trust Store is a list of trusted X.509 certificates of Root Certificate Authorities that are used by SAP Busniess Technology Platform (SAP BTP), and which shall be used and regularly updated by customers in their SAP BTP clients to ensure disruption-free TLS communication.

Two folders are provided as **required** and **optional**.

## The `REQUIRED` SAP BTP Trust Store

This is the list of Root CAs that each component running on SAP BTP and all consumers of SAP BTP **MUST** trust.

Customers **SHOULD** use this trust store in their own applications to make sure they are compatible with current and future Public Key Infrastructures (PKIs) used by SAP BTP.

All Root CAs are approved by SAP.

### Typical use cases:

1. Your client connects with SAP BTP services.

2. You run a custom domain with mutual TLS that allows connections from SAP BTP clients.

3. You run a custom domain with mutual TLS that allows connections from non-SAP-BTP clients, which got a client certificate from an SAP BTP certificate service.


## The `OPTIONAL` SAP BTP Trust Store

This is the complete list of Root CAs approved by SAP.

All `REQUIRED` Root CAs are contained.

### Typical use cases:

1. Your client connects with SAP BTP and non-SAP-BTP services, and the assumption is that the target server certificate was issued by one of these PKIs.

2. You run a custom domain with mutual TLS that allows connections from SAP BTP and non-SAP-BTP clients, and the assumption is that the client certificate was issued by one of these PKIs.


## Version of SAP BTP Trust Store

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

SAP BTP Trust Store is changed whenever a new PKI is introduced in SAP BTP, or an old PKI that was used in the past is not active anymore.

New PKIs are added with sufficient lead time, before they are actively used by SAP BTP. It is important to consume SAP BTP Trust Store regularly, at least once per quarter, to make sure certificates issued by such new PKI CA can be validated successfully.

Old PKIs are removed after sufficient follow-up time, i.e., when the all issued certificates used in SAP BTP are expired, and the PKI CAs are not actively used by SAP BTP anymore.

### Consuming the stores

All PEM files are plain text files, they do not contain private keys. You may have to concatenate all single `.pem` files of the desired store flavour if your target environment is expecting a single CA file (`.pem` or `.crt`) with multiple entries.

The store file type PSE (for ABAP or HANA, resp. SAP CommonCryptoLib) is not password protected, they do not contain private keys.

The store file types JKS (for Java) and P12 (for Java >= 9) are password protected by design, although they do not contain private keys.

> The key store password for all JKS and P12 files is `changeit`.
