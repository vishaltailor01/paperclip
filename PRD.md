# Product Requirements Document: Bring Your Own Key (BYOK) LLM Provider Model

## 1. Product Summary

### 1.1 Product Name

Paperclip BYOK LLM Provider Model

### 1.2 Purpose

The Bring Your Own Key model allows each customer, organisation, or company workspace to connect their own approved LLM provider credentials and use those credentials for agent execution, QA workflows, task generation, planning, summaries, and other AI-assisted actions.

The platform should not require the product owner to pay for every customer’s LLM usage during early commercialisation. Instead, customers control their own LLM accounts, provider billing, model choice, data-processing settings, and enterprise procurement requirements.

### 1.3 Business Objective

The BYOK model reduces operating-cost risk, shortens the path to paid pilots, and makes the product more acceptable to technical and enterprise customers that already have approved AI vendors.

The product monetises the platform layer rather than raw model usage. Customers pay for the dashboard, orchestration, QA workflows, governance, approval flows, audit trails, templates, integrations, hosting, and support while paying their LLM provider directly.

### 1.4 Target Product Direction

The initial commercial direction is a QA-focused AI agent control console built on top of Paperclip concepts. The BYOK provider layer should be generic enough to support broader Paperclip agent orchestration later.

Primary positioning:

> A governed AI-agent dashboard where teams connect their own LLM provider and safely run supervised agent workflows with budgets, approvals, usage visibility, and audit trails.

---

## 2. Problem Statement

Early SaaS products that rely on LLMs face a major commercial risk: unpredictable token usage can exceed subscription revenue. Customers may run large prompts, long agent sessions, retries, or expensive models, creating direct cost exposure for the platform owner.

Enterprise buyers also often prefer to use their own approved providers, such as Azure OpenAI, Anthropic, OpenAI, Google Gemini, AWS Bedrock, OpenRouter, or an internal model gateway. They may already have procurement, security, data-retention, and compliance controls around those providers.

The product needs a safe model where:

- Customers can plug in their own LLM credentials.
- Customers pay their provider directly.
- The platform controls workflow, governance, and auditability.
- The platform shows usage estimates and enforces budget controls.
- The platform does not expose or log customer API keys.
- The platform can later add managed AI credits as a premium option.

---

## 3. Goals and Non-Goals

### 3.1 Goals

- Allow customers to add, test, update, disable, rotate, and delete their own LLM provider credentials.
- Support organisation-level and company-level provider configuration.
- Allow agents and workflows to select an approved provider and model.
- Enforce per-provider, per-agent, per-project, and per-workflow budget limits.
- Provide clear usage visibility for tokens, estimated cost, provider, model, agent, project, and task.
- Prevent raw API keys from being displayed after creation.
- Encrypt provider credentials at rest.
- Avoid logging secrets in application logs, audit logs, telemetry, errors, traces, or browser output.
- Add audit events for credential lifecycle and LLM usage actions.
- Support pilot-friendly provider setup with minimal friction.
- Support enterprise-friendly controls such as RBAC, provider restrictions, and model allowlists.

### 3.2 Non-Goals for MVP

- The platform will not resell LLM credits in the MVP.
- The platform will not guarantee exact provider billing reconciliation in the MVP.
- The platform will not support every LLM provider at launch.
- The platform will not build a full enterprise SAML/SCIM system in the MVP.
- The platform will not expose raw prompts or sensitive payloads in public telemetry.
- The platform will not automatically optimise prompts for cheapest possible execution in the MVP.
- The platform will not support full fine-tuning or model training workflows in the MVP.

---

## 4. Target Users

### 4.1 Organisation Owner

The organisation owner manages workspace billing, security settings, approved providers, and high-level usage policies.

Needs:

- Connect organisation-approved provider credentials.
- Control which teams can use which providers.
- Set global spend and usage limits.
- View usage across projects and agents.
- Rotate or revoke credentials.
- Export audit events.

### 4.2 Workspace or Company Admin

The company admin configures provider usage for a specific company, project, or business workflow.

Needs:

- Choose provider and model defaults.
- Configure agent-level model access.
- Set workflow budgets.
- Test provider connectivity.
- Diagnose provider failures.

### 4.3 QA Lead / Operator

The QA lead uses AI workflows but may not manage provider credentials directly.

Needs:

- Run approved QA workflows.
- See whether a task will use AI.
- Understand estimated usage.
- Review generated outputs before export.
- Avoid accidentally exceeding budgets.

### 4.4 Agent Actor

An agent is a system actor that uses the provider configuration to perform tasks.

Needs:

- Receive only approved provider access.
- Use only allowed models.
- Respect budget limits.
- Fail safely when no provider is configured.

### 4.5 Auditor / Security Reviewer

The auditor verifies that credentials, usage, and sensitive actions are controlled.

Needs:

- See who added or changed provider settings.
- See which provider and model were used for each run.
- Confirm that raw API keys are not exposed.
- Export provider and usage audit events.

---

## 5. Primary Use Cases

### 5.1 Connect an OpenAI API Key

An organisation owner opens AI Provider Settings, selects OpenAI, enters an API key, chooses allowed models, tests the connection, saves it, and sets a monthly usage cap.

Success result:

- Provider status becomes Active.
- Raw key is no longer visible.
- Audit event records provider creation without storing the key.
- Agents can use the provider if allowed by policy.

### 5.2 Connect an Anthropic API Key

An admin selects Anthropic, enters a Claude API key, chooses allowed models, tests the connection, saves it, and assigns it to QA workflows.

Success result:

- QA workflows can use Anthropic models.
- Usage dashboard shows provider and model usage.

### 5.3 Use Azure OpenAI for Enterprise Customers

An enterprise customer enters Azure OpenAI endpoint, deployment name, API version, and key.

Success result:

- The platform calls the customer’s Azure deployment.
- Model selection maps to Azure deployment names.
- Usage is attributed to the enterprise tenant.

### 5.4 Run a QA Test Case Generation Task

A QA lead creates a task to generate regression test cases from a user story. The system uses the company’s default provider and model.

Success result:

- The task runs only if provider status is Active.
- The estimated cost is below policy limits.
- The output is created for review.
- Usage is logged against the task, project, agent, model, and provider.

### 5.5 Budget Limit Blocks a Run

An agent attempts to start a task, but the company’s monthly usage limit has been reached.

Success result:

- The run is blocked before calling the provider.
- The user sees a clear error.
- The audit log records the budget-block event.
- No LLM cost is incurred.

### 5.6 Rotate a Provider Key

An admin updates an existing provider credential with a new key.

Success result:

- New key is encrypted and stored.
- Old encrypted credential is replaced or versioned according to retention policy.
- Audit event records credential rotation.
- Existing agents use the new credential for future runs.

### 5.7 Delete a Provider Credential

An owner deletes a provider credential.

Success result:

- Future runs using that provider fail safely.
- Existing historical usage remains visible.
- Raw key is destroyed or marked deleted according to secure deletion capability.
- Audit event records deletion.

---

## 6. MVP Scope

### 6.1 Provider Support

MVP should support:

1. OpenAI
2. Anthropic
3. Azure OpenAI

Optional after MVP:

- Google Gemini
- OpenRouter
- AWS Bedrock
- Local OpenAI-compatible endpoint
- Ollama or internal model gateway

### 6.2 Provider Settings UI

The MVP must include an AI Providers settings page where authorised users can:

- Add provider.
- Select provider type.
- Enter credential fields.
- Select allowed models.
- Set default model.
- Set provider display name.
- Test connection.
- Save provider.
- Disable provider.
- Rotate key.
- Delete provider.
- View masked credential metadata.

### 6.3 Workflow Integration

The MVP must allow:

- Organisation default provider.
- Company default provider.
- Agent-specific provider override.
- Workflow/task provider override when permitted.
- Safe fallback when no provider is configured.

### 6.4 Usage Visibility

The MVP must show:

- Provider name.
- Model name.
- Agent name.
- Task or run name.
- Prompt tokens when provider returns token data.
- Completion tokens when provider returns token data.
- Total tokens when provider returns token data.
- Estimated cost when pricing is configured.
- Run status.
- Timestamp.

### 6.5 Budget Controls

The MVP must support:

- Monthly company-level estimated cost limit.
- Per-run estimated cost limit.
- Per-agent monthly estimated cost limit.
- Hard stop when budget is exceeded.
- Warning threshold at configurable percentage.
- Clear UI state when usage is blocked.

### 6.6 Audit Events

The MVP must audit:

- Provider created.
- Provider updated.
- Provider disabled.
- Provider enabled.
- Provider key rotated.
- Provider deleted.
- Provider connection tested.
- LLM run started.
- LLM run completed.
- LLM run failed.
- LLM run blocked by budget.
- LLM run blocked by missing provider.
- LLM run blocked by permissions.

Audit events must never include raw API keys.

---

## 7. Functional Requirements

### 7.1 Provider Management

#### PRD-BYOK-FR-001: Add Provider

An authorised admin can add a new provider configuration.

Required fields:

- Provider type.
- Provider display name.
- Credential fields.
- Allowed models.
- Default model.
- Scope.

Scope options:

- Organisation.
- Company.
- Project.

Acceptance criteria:

- Saving a valid provider creates an encrypted credential record.
- UI shows only masked key metadata after save.
- Provider appears in provider list.
- Audit event is created.

#### PRD-BYOK-FR-002: Test Provider Connection

An authorised admin can test credentials before or after saving.

Acceptance criteria:

- Test confirms whether the provider can authenticate.
- Test does not log raw credentials.
- Test uses a minimal low-cost request where possible.
- Failure message is useful but does not leak secrets.
- Audit event records that a test was performed.

#### PRD-BYOK-FR-003: Update Provider Metadata

An authorised admin can update display name, allowed models, default model, budget settings, and enabled status.

Acceptance criteria:

- Updates apply only to future runs.
- Audit event records changed fields without secrets.
- Existing usage history remains unchanged.

#### PRD-BYOK-FR-004: Rotate Provider Credential

An authorised admin can replace the secret credential.

Acceptance criteria:

- New key is validated if the user chooses Test and Save.
- Old key is not visible.
- Audit event records key rotation.
- Future runs use new key.

#### PRD-BYOK-FR-005: Delete Provider

An authorised owner can delete a provider.

Acceptance criteria:

- Provider cannot be selected for new runs.
- Existing historical usage remains visible.
- Dependent agents show configuration warning.
- Audit event records deletion.

---

### 7.2 Provider Selection

#### PRD-BYOK-FR-006: Default Provider Resolution

The system must resolve provider in this order:

1. Task override, if allowed.
2. Workflow override.
3. Agent default.
4. Project default.
5. Company default.
6. Organisation default.
7. No provider configured.

Acceptance criteria:

- Resolution path is visible in debug/admin view.
- Run fails safely if no provider is available.
- Run does not silently fall back to a different provider unless configured.

#### PRD-BYOK-FR-007: Model Allowlist

Admins can restrict which models are available for each provider.

Acceptance criteria:

- Disallowed models cannot be selected in UI.
- API requests using disallowed models are rejected.
- Audit event records blocked model attempts.

#### PRD-BYOK-FR-008: Agent-Level Provider Policy

Admins can assign allowed providers and models to an agent.

Acceptance criteria:

- Agent cannot use unapproved provider.
- Agent cannot use unapproved model.
- Policy is enforced server-side.

---

### 7.3 Execution and Usage Metering

#### PRD-BYOK-FR-009: LLM Run Record

Every LLM call must create a usage record.

Required fields:

- Organisation ID.
- Company ID.
- Project ID, if available.
- Task ID, if available.
- Agent ID, if available.
- Provider ID.
- Provider type.
- Model.
- Run ID.
- Status.
- Started timestamp.
- Completed timestamp.
- Prompt token count, if available.
- Completion token count, if available.
- Total token count, if available.
- Estimated cost, if available.
- Error category, if failed.

Acceptance criteria:

- Usage records exist for successful and failed calls where possible.
- Raw prompts are not stored by default unless explicit trace retention is enabled.
- Usage is queryable by date range and scope.

#### PRD-BYOK-FR-010: Cost Estimation

The system estimates cost using configured pricing tables.

Acceptance criteria:

- Cost estimation works for supported models.
- Unknown model pricing is shown as Unknown rather than zero.
- Admin can configure custom pricing for Azure and custom endpoints.
- UI clearly labels cost as estimated.

#### PRD-BYOK-FR-011: Budget Pre-Check

Before making a provider call, the system checks relevant budgets.

Acceptance criteria:

- Calls are blocked if hard budget is exceeded.
- Calls are allowed if under budget.
- Warning threshold does not block unless configured.
- Budget-blocked runs do not call the provider.

#### PRD-BYOK-FR-012: Budget Post-Update

After a provider call, the system updates usage totals.

Acceptance criteria:

- Usage totals update after successful calls.
- Failed calls update usage if provider returned token usage or billable data.
- Budget warnings are generated when thresholds are crossed.

---

### 7.4 Permissions and RBAC

#### PRD-BYOK-FR-013: Provider Admin Permissions

Only authorised roles can manage provider credentials.

Minimum permission set:

- provider:create
- provider:read
- provider:update
- provider:rotate_secret
- provider:disable
- provider:delete
- provider:test
- provider:assign

Acceptance criteria:

- Non-admin users cannot create or view credential forms.
- Non-admin users cannot rotate or delete credentials.
- Provider usage can be visible to operators without exposing secrets.

#### PRD-BYOK-FR-014: Secret Redaction

Provider secrets must be redacted everywhere except initial entry.

Acceptance criteria:

- API responses never return raw secret values.
- UI never displays raw secret values after save.
- Logs never contain raw secret values.
- Error messages never contain raw secret values.
- Audit events never contain raw secret values.

---

### 7.5 Error Handling

#### PRD-BYOK-FR-015: Missing Provider Error

If no provider is configured, the user should see an actionable message.

Example message:

"No active AI provider is configured for this workspace. Ask an admin to connect an OpenAI, Anthropic, or Azure OpenAI key."

Acceptance criteria:

- Error is shown before any run starts.
- User sees who can fix it or where to fix it if authorised.

#### PRD-BYOK-FR-016: Invalid Credential Error

If provider authentication fails, the system shows a safe message.

Acceptance criteria:

- Error does not expose provider key.
- Provider may be marked Degraded after repeated failures.
- Admin receives clear action to rotate or test key.

#### PRD-BYOK-FR-017: Rate Limit Error

If provider rate limits the customer, the system shows provider-specific guidance.

Acceptance criteria:

- Run status becomes Failed or Retry Scheduled according to workflow policy.
- Error category is rate_limit.
- UI explains that the customer’s provider account returned a rate limit.

#### PRD-BYOK-FR-018: Provider Outage Error

If provider is unavailable, the system records provider_unavailable.

Acceptance criteria:

- Error is visible on run detail.
- Retry policy is applied if configured.
- No automatic provider switch occurs unless admin enabled fallback.

---

## 8. Non-Functional Requirements

### 8.1 Security

- Secrets must be encrypted at rest.
- Secrets must be decrypted only server-side at execution time.
- Secrets must never be sent to the browser after save.
- Secrets must never be included in logs, traces, audit events, telemetry, or error payloads.
- Provider actions must be protected by server-side permission checks.
- Provider use must be scoped by organisation, company, project, and agent policy.
- All provider credential mutations must be audited.

### 8.2 Privacy

- Customer prompts and outputs should not be used for product telemetry by default.
- Customer API keys must not be shared with third parties except the selected provider call path.
- Usage analytics should use metadata only unless trace retention is explicitly enabled.

### 8.3 Reliability

- Provider failures should not crash agent execution workers.
- Provider failure states must be visible.
- Budget pre-checks must be atomic enough to avoid obvious runaway spend.
- Long-running agent workflows must re-check budget before each provider call.

### 8.4 Performance

- Provider settings page should load within 2 seconds for normal workspaces.
- Usage dashboard should support filtering by provider, model, project, agent, and date range.
- Provider call wrapper should add minimal overhead compared with direct provider calls.

### 8.5 Scalability

- Usage records should be append-only and queryable by time range.
- Aggregated usage tables or materialised summaries may be introduced when raw usage grows.
- Provider credentials must be isolated by tenant scope.

### 8.6 Compliance Readiness

- Audit logs should be exportable.
- Credential lifecycle events should be retained according to tenant retention settings.
- The system should support future SOC 2 evidence collection.

---

## 9. Data Model Requirements

### 9.1 llm_provider_configs

Purpose:

Stores non-secret provider configuration.

Suggested fields:

- id
- organisation_id
- company_id nullable
- project_id nullable
- provider_type
- display_name
- status
- scope
- default_model
- allowed_models json
- endpoint_url nullable
- azure_deployment_name nullable
- azure_api_version nullable
- pricing_profile_id nullable
- monthly_budget_limit nullable
- per_run_budget_limit nullable
- warning_threshold_percent nullable
- created_by
- updated_by
- created_at
- updated_at
- disabled_at nullable
- deleted_at nullable

### 9.2 llm_provider_secrets

Purpose:

Stores encrypted provider secret material.

Suggested fields:

- id
- provider_config_id
- secret_ciphertext
- secret_key_version
- secret_fingerprint
- secret_last_four nullable
- created_by
- rotated_by nullable
- created_at
- rotated_at nullable
- deleted_at nullable

Important:

- The raw secret must never be stored outside encrypted storage.
- The fingerprint can support duplicate detection without revealing the key.

### 9.3 llm_usage_events

Purpose:

Stores per-call usage.

Suggested fields:

- id
- organisation_id
- company_id
- project_id nullable
- task_id nullable
- issue_id nullable
- agent_id nullable
- run_id nullable
- provider_config_id
- provider_type
- model
- status
- prompt_tokens nullable
- completion_tokens nullable
- total_tokens nullable
- estimated_cost_minor nullable
- currency
- request_started_at
- request_completed_at nullable
- duration_ms nullable
- error_category nullable
- error_message_redacted nullable
- created_at

### 9.4 llm_budget_snapshots

Purpose:

Stores precomputed budget usage for fast reads.

Suggested fields:

- id
- organisation_id
- company_id nullable
- project_id nullable
- agent_id nullable
- provider_config_id nullable
- period_start
- period_end
- estimated_cost_minor
- token_count
- run_count
- updated_at

### 9.5 llm_pricing_profiles

Purpose:

Stores pricing metadata for estimation.

Suggested fields:

- id
- provider_type
- model
- input_cost_per_million_tokens_minor
- output_cost_per_million_tokens_minor
- currency
- source
- effective_from
- created_at
- updated_at

---

## 10. UI Requirements

### 10.1 AI Providers List Page

Must show:

- Provider display name.
- Provider type.
- Scope.
- Status.
- Default model.
- Allowed model count.
- Monthly budget.
- Usage this month.
- Last tested time.
- Last used time.
- Actions menu.

Actions:

- Add provider.
- Test connection.
- Edit settings.
- Rotate key.
- Disable.
- Delete.

### 10.2 Add Provider Wizard

Steps:

1. Select provider type.
2. Enter credential details.
3. Select models.
4. Configure budget.
5. Test connection.
6. Save.

### 10.3 Provider Detail Page

Must show:

- Provider metadata.
- Masked credential information.
- Allowed models.
- Budget controls.
- Recent usage.
- Recent errors.
- Audit events.
- Assigned agents and workflows.

### 10.4 Usage Dashboard

Filters:

- Date range.
- Provider.
- Model.
- Company.
- Project.
- Agent.
- Task status.

Metrics:

- Estimated cost.
- Total tokens.
- Run count.
- Failed runs.
- Budget remaining.
- Top agents by usage.
- Top projects by usage.

### 10.5 Run Detail Usage Panel

Each AI-assisted run should show:

- Provider.
- Model.
- Token usage.
- Estimated cost.
- Budget policy applied.
- Error category if failed.
- Whether prompt/output trace was retained.

---

## 11. API Requirements

### 11.1 Provider Config APIs

Required operations:

- Create provider config.
- List provider configs.
- Read provider config.
- Update provider config.
- Disable provider config.
- Delete provider config.
- Test provider connection.
- Rotate provider secret.
- List provider usage.
- List provider audit events.

### 11.2 Execution API Integration

Agent and workflow execution must use a single provider gateway service rather than calling providers directly from scattered code paths.

Gateway responsibilities:

- Resolve provider.
- Check permissions.
- Check model allowlist.
- Check budget.
- Decrypt secret at execution time.
- Call provider.
- Record usage.
- Redact errors.
- Return normalised response.

---

## 12. Provider Gateway Requirements

### 12.1 Normalised Request

The provider gateway should accept a normalised request containing:

- messages or prompt payload.
- model.
- provider preference.
- task/run context.
- agent context.
- max tokens.
- temperature.
- tool options if supported.
- tracing preference.

### 12.2 Normalised Response

The provider gateway should return:

- text output.
- structured output if applicable.
- provider type.
- model.
- usage tokens.
- estimated cost.
- provider request ID if available.
- finish reason.
- error category if failed.

### 12.3 Error Categories

Standard categories:

- missing_provider
- provider_disabled
- permission_denied
- model_not_allowed
- budget_exceeded
- invalid_credentials
- rate_limit
- provider_unavailable
- timeout
- context_length_exceeded
- content_filter
- unknown_provider_error

---

## 13. Security Requirements

### 13.1 Secret Encryption

- Use envelope encryption where possible.
- Use environment or KMS-backed master key.
- Support key rotation plan.
- Store only ciphertext, fingerprint, and non-sensitive metadata.

### 13.2 Secret Handling

- Never return raw secrets in API responses.
- Never log request bodies containing credentials.
- Redact common key patterns in logs.
- Use write-only input fields in UI.
- Show only provider type and masked metadata after save.

### 13.3 Access Control

- Provider credentials are admin-managed.
- Operators can view provider availability and usage, but not secrets.
- Agents can use credentials only through the provider gateway.
- Agents cannot retrieve credentials directly.

### 13.4 Auditability

- Every credential lifecycle event must be audited.
- Every provider use must be linked to an actor and run context.
- Audit logs must be immutable enough for enterprise review.

### 13.5 Tenant Isolation

- Provider configs must be scoped to tenant/company/project.
- Cross-tenant provider access must be impossible server-side.
- Usage queries must enforce tenant scope.

---

## 14. Budget and Cost Controls

### 14.1 Budget Types

- Organisation monthly budget.
- Company monthly budget.
- Project monthly budget.
- Agent monthly budget.
- Provider monthly budget.
- Per-run budget.

### 14.2 Budget Actions

When warning threshold is crossed:

- Show warning in UI.
- Add activity event.
- Optionally notify admins.

When hard limit is crossed:

- Block new provider calls.
- Pause relevant workflows if configured.
- Create budget-block event.
- Show remediation action.

### 14.3 Cost Estimation Caveat

Costs are estimates based on provider-reported token usage and configured pricing. Customer’s actual provider invoice remains the source of truth.

The UI must clearly state this.

---

## 15. Notifications

MVP notifications:

- Provider key invalid.
- Provider rate limited.
- Budget warning threshold crossed.
- Budget hard limit reached.
- Provider disabled.
- Provider deleted while assigned agents exist.

Notification channels for MVP:

- In-app notification.
- Dashboard warning.

Future channels:

- Email.
- Slack.
- Microsoft Teams.
- Webhook.

---

## 16. QA and Testing Requirements

### 16.1 Unit Tests

Test:

- Provider resolution logic.
- Model allowlist enforcement.
- Budget pre-check logic.
- Cost estimation.
- Secret redaction helpers.
- Error categorisation.

### 16.2 API Tests

Test:

- Provider CRUD permissions.
- Non-admin cannot access credential endpoints.
- Raw secret never appears in response.
- Provider test handles success and failure safely.
- Deleted provider cannot be used.

### 16.3 Integration Tests

Test:

- OpenAI provider happy path.
- Anthropic provider happy path.
- Azure OpenAI provider happy path.
- Invalid key path.
- Rate limit mocked path.
- Budget exceeded path.

### 16.4 E2E Tests

Test user journeys:

1. Admin adds provider and tests connection.
2. Operator runs QA task using default provider.
3. Budget threshold warning appears.
4. Budget hard stop blocks run.
5. Admin rotates key.
6. Admin deletes provider and dependent agent shows warning.

### 16.5 Security Tests

Test:

- API key is never returned after save.
- API key is not logged on validation failure.
- Cross-tenant provider access is blocked.
- Agent cannot retrieve raw credential.
- Operator cannot rotate credential.

---

## 17. Analytics and Success Metrics

### 17.1 Product Metrics

- Number of organisations with active provider configured.
- Provider setup completion rate.
- Provider test success rate.
- First successful AI task completion rate.
- Number of AI runs per active customer.
- Percentage of blocked runs due to missing provider.
- Percentage of blocked runs due to budget.
- Provider failure rate by provider type.

### 17.2 Business Metrics

- Paid pilot conversion rate.
- Trial-to-paid conversion rate.
- Monthly recurring revenue.
- Gross margin impact from BYOK versus managed AI credits.
- Support tickets per provider setup.
- Churn reason related to provider setup complexity.

### 17.3 Security Metrics

- Number of credential rotations.
- Number of invalid-key events.
- Number of permission-denied events.
- Number of cross-scope access attempts blocked.

---

## 18. Rollout Plan

### Phase 1: Internal Prototype

Deliver:

- Provider config data model.
- OpenAI support.
- Basic settings UI.
- Provider gateway.
- Usage record creation.
- Manual budget limit.

Exit criteria:

- Internal user can add OpenAI key and run a QA task.
- Usage is recorded.
- Key is not exposed after save.

### Phase 2: Paid Pilot MVP

Deliver:

- OpenAI, Anthropic, Azure OpenAI support.
- Provider test connection.
- Usage dashboard.
- Company and agent defaults.
- Monthly and per-run budget controls.
- Audit events.

Exit criteria:

- First pilot customer can connect their own key.
- Customer can run QA workflows.
- Customer can see estimated usage.
- Customer can rotate or delete provider key.

### Phase 3: Team Beta

Deliver:

- RBAC polish.
- Model allowlists.
- Notifications.
- More usage filters.
- Exportable audit log.
- Better provider diagnostics.

Exit criteria:

- Multiple users can safely operate under one organisation.
- Admins can control provider access.
- Operators can run tasks without seeing secrets.

### Phase 4: Enterprise Readiness

Deliver:

- KMS-backed secret encryption.
- Advanced audit exports.
- Provider policy templates.
- SSO integration support.
- External vault support.
- More custom providers.

Exit criteria:

- Enterprise customer can pass vendor/security review for BYOK usage.

---

## 19. Risks and Mitigations

### 19.1 Risk: Users Find API Key Setup Confusing

Mitigation:

- Add guided setup steps.
- Add provider-specific help text.
- Add Test Connection.
- Add demo mode with sample output.

### 19.2 Risk: Customer Blames Platform for Provider Bill

Mitigation:

- Clearly show estimated usage.
- Add budget limits.
- State that provider invoice is source of truth.
- Provide per-task usage records.

### 19.3 Risk: Secret Leakage

Mitigation:

- Encrypt at rest.
- Redact logs.
- Never return raw keys.
- Add automated tests for redaction.
- Restrict credential permissions.

### 19.4 Risk: Provider API Changes

Mitigation:

- Use provider adapter layer.
- Add integration tests.
- Version provider clients.
- Monitor provider error rates.

### 19.5 Risk: Cost Estimation Is Inaccurate

Mitigation:

- Label as estimate.
- Allow custom pricing profiles.
- Update pricing profiles regularly.
- Show unknown when pricing is unavailable.

### 19.6 Risk: Agent Abuse or Runaway Loops

Mitigation:

- Budget pre-check before every call.
- Per-run limits.
- Agent-level budgets.
- Kill switch.
- Approval gates for high-cost workflows.

---

## 20. Open Questions

- Should provider credentials be stored at organisation level only for MVP, or also company level?
- Should prompt/output trace retention be disabled by default for all BYOK customers?
- Should admins be able to configure fallback providers?
- Should budget checks use estimated pre-call token budgets or only historical spend?
- Should Azure OpenAI be required for enterprise pilots before SAML/SSO is available?
- Which QA workflows should be prioritised for BYOK launch?
- Should custom OpenAI-compatible endpoints be included in MVP or after paid pilots?

---

## 21. Commercial Packaging

### 21.1 Starter BYOK Plan

Target:

Solo users and early testers.

Includes:

- One organisation.
- One provider credential.
- Basic QA workflows.
- Basic usage history.
- Customer pays provider directly.

### 21.2 Team BYOK Plan

Target:

Small QA or engineering teams.

Includes:

- Multiple users.
- Multiple projects.
- Multiple provider credentials.
- Agent-level budgets.
- Review and approval workflow.
- Usage dashboard.
- Audit history.

### 21.3 Business BYOK Plan

Target:

Companies needing governance.

Includes:

- RBAC.
- Provider model allowlists.
- Audit exports.
- Advanced budgets.
- Integrations.
- Priority support.

### 21.4 Enterprise BYOK Plan

Target:

Security-conscious customers.

Includes:

- SSO.
- KMS or external vault option.
- Advanced audit retention.
- Private deployment option.
- Custom provider gateway.
- Support SLA.

---

## 22. Launch Readiness Checklist

- Provider settings UI completed.
- OpenAI provider tested.
- Anthropic provider tested.
- Azure OpenAI provider tested.
- Secrets encrypted at rest.
- Secrets redacted from logs and API responses.
- Provider gateway integrated with at least one QA workflow.
- Usage events recorded.
- Budget hard stop implemented.
- Audit events implemented.
- Permission checks implemented.
- E2E tests passing.
- Setup guide written.
- Customer-facing BYOK terms written.
- Support runbook written.

---

## 23. Future Enhancements

- Managed AI credits as a premium option.
- Provider fallback policies.
- Prompt optimisation by provider.
- Model benchmarking dashboard.
- Custom model gateway.
- AWS Bedrock support.
- Google Gemini support.
- OpenRouter support.
- Local model support.
- External vault integrations.
- Usage anomaly detection.
- Forecasted monthly spend.
- Per-feature cost attribution.
- Slack and Teams notifications.
- Enterprise policy engine.

---

## 24. Final Recommendation

The BYOK model should be the default commercial model for the first paid pilots and SaaS beta. It protects platform margin, gives customers control, and makes enterprise adoption easier.

The product should later add managed AI credits as an optional convenience tier, but only after usage metering, budget enforcement, abuse prevention, and billing controls are mature.
