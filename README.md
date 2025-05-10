# Control Name: Secure Key Storage
Control ID: KMS-01
Category: Cryptographic Protections
Framework Alignment: NIST SP 800-53 Rev. 5: SC-12, SC-28, SC-28(1), SC-34
Control Type: Technical
Control Class: Preventive

## Control Definition:
The organization shall implement secure, centralized key storage to protect cryptographic keys used for data encryption, authentication, or digital signing. Key storage must be isolated from application code and data, enforce strict access controls, and provide capabilities for key rotation, versioning, and revocation. All cryptographic keys must be stored using a FIPS 140-2 or higher validated hardware security module (HSM) or equivalent cloud-native Key Management Service (KMS).

## Control Objectives:
- Prevent unauthorized access, disclosure, or tampering of cryptographic keys.
- Ensure the confidentiality, integrity, and availability of cryptographic materials.
- Enable secure key lifecycle management (generation, use, rotation, deactivation, destruction).
- Enforce least privilege and role-based access to key materials.
- Provide auditability and visibility into key usage.

## Implementation Requirements:
1. Key Management System (KMS):
    - Use a cloud-native or enterprise KMS (e.g., AWS KMS, Azure Key Vault, Google Cloud KMS) that supports customer-managed keys (CMKs).
    - Keys must be logically separated per application and data classification requirements.
2. Access Control:
    - Implement fine-grained access policies (e.g., IAM roles, ACLs) for key usage.
    - Require MFA for privileged access to key management interfaces.
3. Logging & Monitoring:
    - Enable detailed logging of key usage, access attempts, and administrative changes.
    - Integrate logs with SIEM for real-time monitoring and anomaly detection.

## Evidence of Compliance:
- Configuration and architecture diagrams showing use of secure KMS.
- IAM policy definitions restricting key access.
- Key rotation schedules and logs.
- Audit logs demonstrating key use and access.
- FIPS 140-2 validation certificates (or CSP compliance attestations).

```mermaid
    C4Context
      title System Context diagram for Internet Banking System
      Enterprise_Boundary(b0, "BankBoundary0") {
        Person(customerA, "Banking Customer A", "A customer of the bank, with personal bank accounts.")
        Person(customerB, "Banking Customer B")
        Person_Ext(customerC, "Banking Customer C", "desc")

        Person(customerD, "Banking Customer D", "A customer of the bank, <br/> with personal bank accounts.")

        System(SystemAA, "Internet Banking System", "Allows customers to view information about their bank accounts, and make payments.")

        Enterprise_Boundary(b1, "BankBoundary") {

          SystemDb_Ext(SystemE, "Mainframe Banking System", "Stores all of the core banking information about customers, accounts, transactions, etc.")

          System_Boundary(b2, "BankBoundary2") {
            System(SystemA, "Banking System A")
            System(SystemB, "Banking System B", "A system of the bank, with personal bank accounts. next line.")
          }

          System_Ext(SystemC, "E-mail system", "The internal Microsoft Exchange e-mail system.")
          SystemDb(SystemD, "Banking System D Database", "A system of the bank, with personal bank accounts.")

          Boundary(b3, "BankBoundary3", "boundary") {
            SystemQueue(SystemF, "Banking System F Queue", "A system of the bank.")
            SystemQueue_Ext(SystemG, "Banking System G Queue", "A system of the bank, with personal bank accounts.")
          }
        }
      }

      BiRel(customerA, SystemAA, "Uses")
      BiRel(SystemAA, SystemE, "Uses")
      Rel(SystemAA, SystemC, "Sends e-mails", "SMTP")
      Rel(SystemC, customerA, "Sends e-mails to")

      UpdateElementStyle(customerA, $fontColor="red", $bgColor="grey", $borderColor="red")
      UpdateRelStyle(customerA, SystemAA, $textColor="blue", $lineColor="blue", $offsetX="5")
      UpdateRelStyle(SystemAA, SystemE, $textColor="blue", $lineColor="blue", $offsetY="-10")
      UpdateRelStyle(SystemAA, SystemC, $textColor="blue", $lineColor="blue", $offsetY="-40", $offsetX="-50")
      UpdateRelStyle(SystemC, customerA, $textColor="red", $lineColor="red", $offsetX="-50", $offsetY="20")

      UpdateLayoutConfig($c4ShapeInRow="3", $c4BoundaryInRow="1")


```
