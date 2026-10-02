# Entra App Risk Investigation Agents

This workspace contains a Microsoft Security Copilot agent package for reviewing
Microsoft Entra application registrations and enterprise applications.

## Included agents

- **Entra App Risk Coordinator** - runs all specialist investigations and
  produces a consolidated, prioritized remediation plan.
- **Entra Permission and Consent Investigator** - reviews configured API
  permissions, delegated consent, application-role grants, admin consent, and
  dangerous permission combinations.
- **Entra Exposure Investigator** - reviews tenant audience, assignment
  requirements, redirect URIs, public-client settings, implicit flows, and
  publisher trust.
- **Entra Credential and Ownership Investigator** - reviews credential lifetime,
  expiry, rotation, federated identity trust, and ownership accountability.
- **Entra Lifecycle Investigator** - reviews sign-in activity, dormancy,
  duplicate registrations, retained access, and decommissioning readiness.

## Deploy

1. Open Microsoft Security Copilot and go to **Agents**.
2. Select **Create agent** and then **Upload a YAML manifest**.
3. Upload `entra-app-risk-agents.yaml`.
4. Confirm that the built-in **Microsoft Entra** plugin is enabled and
   authorized for the tenant. The manifest declares the `Entra` skillset and
   the agents query it automatically.
5. Review the generated agent definitions and tools.
6. Test each specialist agent before publishing the coordinator.

The manifest intentionally disables scheduled execution by setting
`DefaultPollPeriodSeconds` to `0`. This avoids autonomous changes or recurring
capacity use until the investigation and data-access design has been validated.

## Automatic Entra evidence collection

The agents use the built-in Entra plugin to retrieve application, service
principal, permission, consent, credential, owner, audit, risk, and activity
data. Start an investigation by describing its scope; do not paste tenant
exports into the prompt.

If a required plugin skill or permission is unavailable, the agent reports the
specific failed skill as a data gap and continues with the data it can retrieve.
Optional evidence supplied by the user can still supplement plugin results.

Do not supply client-secret values, certificate private keys, access tokens, or
other credentials. Credential metadata is sufficient.

## Recommended test sequence

1. Test each specialist with a small set of known applications.
2. Confirm that configured permissions are not reported as effective grants
   without consent evidence.
3. Confirm that missing activity is reported as a data gap rather than as an
   unused application.
4. Confirm that `appRoleAssignmentRequired` is evaluated on service principals,
   not application objects.
5. Compare the findings with Entra portal records before publishing.
6. Publish the coordinator only after specialist outputs meet the organization's
   evidence, severity, and remediation standards.

The package analyzes evidence but does not make tenant changes. Remediation
should use owner validation, dependency testing, change control, and rollback
planning.
