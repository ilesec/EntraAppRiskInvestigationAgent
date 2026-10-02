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
4. Review the generated agent definitions and tools.
5. Test each specialist agent before publishing the coordinator.

The manifest intentionally disables scheduled execution by setting
`DefaultPollPeriodSeconds` to `0`. This avoids autonomous changes or recurring
capacity use until the investigation and data-access design has been validated.

## Evidence to provide

The agents are evidence-driven. Supply authenticated tool output or exports with:

- Applications: object ID, app ID, display name, creation date,
  `signInAudience`, `requiredResourceAccess`, redirect URIs, public-client and
  implicit-flow settings, verified publisher, credentials, federated identity
  credentials, and owners.
- Service principals: object ID, app ID, enabled state,
  `appRoleAssignmentRequired`, service principal type, owners, assigned users
  and groups, and app-role assignments.
- Consent: OAuth permission grants and application-role assignments, including
  resource, client, principal, consent type, scopes or roles, and timestamps
  where available.
- Activity: service principal and application sign-ins plus relevant audit
  events. Include the observation period and log-retention boundary.

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
