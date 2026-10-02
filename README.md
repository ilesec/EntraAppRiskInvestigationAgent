# Entra App Risk Investigation Agents

This workspace contains a Microsoft Security Copilot agent for reviewing
Microsoft Entra application registrations and enterprise applications.

## Included analysis

- **Entra App Risk Coordinator** - retrieves Entra evidence, runs all analysis
  stages, and produces a consolidated, prioritized remediation plan.
- **Permission and consent analysis** - reviews configured API
  permissions, delegated consent, application-role grants, admin consent, and
  dangerous permission combinations.
- **Exposure analysis** - reviews tenant audience, assignment
  requirements, redirect URIs, public-client settings, implicit flows, and
  publisher trust.
- **Credential and ownership analysis** - reviews credential lifetime,
  expiry, rotation, federated identity trust, and ownership accountability.
- **Lifecycle analysis** - reviews sign-in activity, dormancy,
  duplicate registrations, retained access, and decommissioning readiness.

## Deploy

1. Open Microsoft Security Copilot and go to **Agents**.
2. Select **Create agent** and then **Upload a YAML manifest**.
3. Upload `entra-app-risk-agents.yaml`.
4. Confirm that the built-in **Microsoft Entra** plugin is enabled and
   authorized for the tenant. The manifest declares the `Entra` skillset and
   the agent queries it automatically.
5. Review the generated agent definition and tools.
6. Test the agent with the **Test Entra access** starter prompt before publishing.

The manifest intentionally disables scheduled execution by setting
`DefaultPollPeriodSeconds` to `0`. This avoids autonomous changes or recurring
capacity use until the investigation and data-access design has been validated.

## Automatic Entra evidence collection

The agent uses the built-in Entra plugin to retrieve application, service
principal, permission, consent, credential, owner, audit, risk, and activity
data. Start an investigation by describing its scope; do not paste tenant
exports into the prompt.

If a required plugin skill or permission is unavailable, the agent reports the
specific failed skill as a data gap and continues with the data it can retrieve.
Optional evidence supplied by the user can still supplement plugin results.

Do not supply client-secret values, certificate private keys, access tokens, or
other credentials. Credential metadata is sufficient.

## Recommended test sequence

1. Run **Test Entra access** and confirm the plugin returns application and
   service principal evidence.
2. Confirm that configured permissions are not reported as effective grants
   without consent evidence.
3. Confirm that missing activity is reported as a data gap rather than as an
   unused application.
4. Confirm that `appRoleAssignmentRequired` is evaluated on service principals,
   not application objects.
5. Compare the findings with Entra portal records before publishing.
6. Publish the agent only after its outputs meet the organization's
   evidence, severity, and remediation standards.

The package analyzes evidence but does not make tenant changes. Remediation
should use owner validation, dependency testing, change control, and rollback
planning.
