# Administration

## Admin Rollout Guide

Plan, configure, and verify a ChatGPT Enterprise rollout across workspace and developer surfaces.

### Overview

This guide addresses administration boundaries including workspace access, local runtime policy for covered capabilities in the ChatGPT desktop app, Codex CLI, and IDE extension, Codex cloud, Platform API access, plugins and apps, and permissions in connected systems.

Note: "Codex Local" in workspace settings is a grouping label for certain local access and access-token controls, not a separate product.

### Step 1: Assign Owners and Choose a Rollout

Assign an owner for each part:

- **Workspace access:** Membership, seats, roles, and supported features
- **Local runtime policy:** Approvals, permission profiles, filesystem and network access
- **Codex cloud:** Hosted environments, repository connections, and cloud runtime policy
- **Connected systems:** Provider-side installation, accounts, and permissions
- **Reporting and compliance:** Analytics access, audit exports, and data handling

Decide which audiences need covered local capabilities in desktop app, Codex CLI, IDE extension, Codex cloud, or combinations.

### Step 2: Configure Workspace Access and Identity

Use ChatGPT workspace membership, seats, groups, and RBAC permissions to grant intended audiences supported features. Verify local client and Codex cloud access against current workspace guidance.

Key resources:
- Manage members, seat types, roles, and access
- Configure role-based access control
- Manage workspace settings
- Groups and provisioning documentation
- Authentication documentation

Test sign-in and feature access with representative members before expanding.

### Step 3: Configure Local Runtime Requirements

Local requirements constrain runtime behavior when users start supported local runs. Deliver `requirements.toml` through supported cloud, device, or system channels.

Example configuration using permission profiles:

```toml
default_permissions = ":workspace"

[allowed_permission_profiles]
":read-only" = true
":workspace" = true
```

To disable Computer Use across browser and desktop surfaces:

```toml
[features]
browser_use = false
browser_use_full_cdp_access = false
browser_use_external = false
in_app_browser = false
computer_use = false
```

### Step 4: Standardize Repository Configuration

Use repository-scoped configuration to share project defaults, rules, and skills:

| Type | Source | Purpose |
|------|--------|---------|
| Configuration | Config basics | Set repository defaults for supported local clients |
| Rules | Rules documentation | Control commands requiring approval outside sandbox |
| Skills | Build skills | Make repository workflows available to clients |

Repository configuration supplies defaults and reusable workflows but cannot grant workspace, model, Platform API, or connected-system access.

### Step 5: Configure Codex Cloud

Plan each boundary:

1. Grant intended audience Codex cloud access through supported workspace controls
2. Install and configure the supported source-system integration
3. Limit repository access in the source system
4. Configure cloud environments, secrets, and internet access
5. Configure optional hosted workflows such as code review
6. Test with representative users having intended workspace and repository permissions

Codex cloud respects repository permissions and protections exposed by connected source systems. Workspace access doesn't bypass those controls.

### Step 6: Configure Plugins and Connected Capabilities

Review plugin installation, bundled skills, app-backed capabilities, app actions, and source-system authorization as separate decisions.

Before including a plugin or skill:

1. Confirm source, accountable owner, intended audience, and review date
2. Review bundled skills, apps, MCP servers, hooks, and data/actions each requires
3. Test with non-sensitive data and least required access
4. Record ownership of re-review and retirement

Plugins are available in ChatGPT Work on the web, ChatGPT Work and Codex in the desktop app, and through Codex CLI plugin browser. They're unavailable in Chat, the IDE extension, or mobile.

### Step 7: Set Up Governance and Observability

Choose the reporting surface matching your question:

- **Workspace analytics:** Interactive ChatGPT workspace and Codex analytics
- **Analytics API:** Programmatic, aggregated reporting through Codex Analytics API
- **Compliance API:** Audit and investigation records
- **Usage limits and spend controls:** When plan-dependent Codex activity consumes ChatGPT workspace credits

Protect the integration boundary:

- Store API keys in the organization's secret-management system
- Limit downstream system access to approved audiences
- Protect exported records according to sensitivity and retention policy

### Step 8: Verify and Maintain the Rollout

Verify every applicable boundary with representative identities:

- ChatGPT workspace membership, seat, and supported role permissions
- Covered local capabilities in desktop app, Codex CLI, and IDE extension
- Codex cloud access, environment configuration, and repository permissions
- Platform API organization and project access for API-key workflows
- Plugin installation, bundled skills, app access, and supported actions
- Connected-system authorization and data access
- Analytics and compliance access for responsible administrators

Record the owner and current procedural source for each control. After initial rollout, review access, connected capabilities, credit use, support feedback, and actual workflows. Adjust scope and guidance when signals change.

---

## ChatGPT Work Admin FAQ

### Overview

ChatGPT Work enables users to delegate longer, multi-step tasks to the AI system. It gathers information from conversations, files, workspace resources, and connected systems; uses approved tools; and creates review-ready artifacts. Work launches July 9, 2026, with web and mobile access starting disabled by default for Enterprise and Edu during a two-week preview period.

### Core Administrative Controls

Administrators manage Work through multiple control layers:

- **Workspace access** via identity controls including SSO, domain verification, and SCIM provisioning
- **Work feature access** using top-level controls and role-based access control (RBAC)
- **Group membership** synchronized through identity providers for automatic access updates
- **Workspace and member roles** determining admin privileges and end-user capabilities
- **Plugin and app governance** through policy, action controls, and approval behavior
- **Source-system permissions** limiting access to authorized content in native applications
- **Approval restrictions** for apps supporting action controls
- **Credits** with per-user monthly limits for eligible Enterprise and Edu workspaces
- **Analytics and reporting** via the Global Admin Console and Compliance API

### Access, Data, Systems, and User Actions

Work respects existing ChatGPT workspace identity, access, and permission controls. Access can synchronize with identity providers through SCIM and group synchronization, enabling centralized management as employees' roles change.

Work accesses data from conversations, uploaded files, workspace resources, and connected systems through plugins. Underlying source systems enforce their own access controls, Work doesn't bypass existing permissions or grant new access in connected applications.

Action risk varies by type. Higher-impact actions (writing, sharing, executing) should be restricted to trusted, reviewed use through roles, narrow permissions, and supported approvals. Lower-impact actions include reading or drafting.

### Compliance

Work uses privacy, security, and data commitments applicable to the customer's ChatGPT workspace. Coverage for data residency, FedRAMP, HIPAA, or BAA varies by plan and configuration and should be confirmed.

Data retention and deletion follow ChatGPT workspace policies and connected application requirements. Connected systems manage their own data lifecycle policies. The Compliance API covers prompts and responses but doesn't track files, actions, or tool calls, with 30 day retention requiring continuous export for longer periods.

### Observability

Workspace analytics show adoption and credit use by user, product, and model. The Compliance API provides user prompts and agent responses. Analytics can identify unusual behavior through signals including unexpected usage spikes, irregular user activity, operational errors, and administrative events.

For Codex activity, analytics and the Analytics API provide adoption metrics. Organizations using local clients can enable OpenTelemetry exports for API requests, errors, and tool decisions (prompt contents are redacted by default).

### Governance

Governance spans three layers: Work access controls determine who can use the personal Work agent; Workspace Agent controls govern who builds and publishes reusable agents; and Codex managed configuration constrains local runtime behavior.

Access can be scoped by workspace roles, identity groups, and administrator defined permissions. Organizations should assign capabilities to groups based on business need rather than providing identical access to all users.

Runtime boundaries depend on the task context. Work on web and mobile is governed separately from desktop and Codex tasks, which follow their own permissions, managed configuration, and approval controls.

### Usage and Cost

ChatGPT Work and Codex share pricing, credits, and usage limits. Consumption varies with model, capability, context size, task duration, tool use, and output size. High variance patterns include frequently run workflows, large information retrieval, multiple app calls, and Codex tasks processing repositories.

Eligible Enterprise and Edu workspaces can set monthly per-user limits through workspace defaults, group defaults, and individual overrides. Users can request increases when enabled. Workspace credit alerts and overage limits provide additional controls.

### Incident and Revocation Controls

Administrators can revoke access through:

- Removing workspace or group access (or via identity provider for SCIM users)
- Disabling or restricting relevant apps
- Revoking shared connections, bots, or service accounts
- Removing Workspace Agents from publication
- Disabling schedules or triggers
- Revoking Codex access tokens, repository connections, and cloud environment access

### Recommended Admin Actions

- Decide rollout scope (pilot vs. broad deployment)
- Confirm RBAC settings match business need
- Review enabled apps and data source permissions
- Set expectations for appropriate use cases
- Review credit controls and usage guidance
- Identify initial high value workflows
- Prepare champions and support teams
- Communicate review and approval expectations
- Monitor adoption and adjust accordingly

---

## Authentication

Sign-in methods for ChatGPT web and Codex clients

### OpenAI authentication

Codex supports two ways to sign in when using OpenAI models:

- Sign in with ChatGPT for subscription access
- Sign in with an API key for usage-based access

The ChatGPT desktop app, Codex CLI, and IDE extension support both sign-in methods for local work. Codex cloud requires signing in with ChatGPT.

Your sign-in method also determines which admin controls and data-handling policies apply.

- When you sign in with ChatGPT, Codex usage follows your ChatGPT workspace permissions, role-based access control (RBAC), and ChatGPT Enterprise retention and residency settings.
- With an API key, usage follows your API organization's retention and data-sharing settings instead.

For managed workspaces, authentication is only one layer of access. Workspace membership and provisioning determine who can sign in, while seats and workspace roles determine which product surfaces and features they can use. For local work in the ChatGPT desktop app, Codex CLI, or IDE extension, permission profiles constrain what the agent can do on the device.

### Sign in with ChatGPT

When you sign in with ChatGPT from the ChatGPT desktop app, Codex CLI, or IDE extension, the sign-in flow opens a browser window. After you sign in, the browser returns your credentials to Codex.

#### ChatGPT web

Open ChatGPT, sign in, and choose the workspace where you want to work. ChatGPT web keeps the authenticated session in your browser.

#### ChatGPT desktop app

On the signed-out screen, select **Continue to sign in**, then complete the browser flow.

#### Codex CLI

Run `codex login`, then complete the browser flow. This is the default authentication path when no valid session is available.

#### IDE extension

On the signed-out screen, select **Sign in with ChatGPT**, then complete the browser flow.

### Sign in with an API key

You can also sign in to the ChatGPT desktop app, Codex CLI, or IDE extension with an API key. Get your API key from the OpenAI dashboard.

#### ChatGPT desktop app

On the signed-out screen, select **Sign in another way**, enter your key, then select **Continue**.

#### Codex CLI

Pipe the key to `codex login` through stdin:

```
printenv OPENAI_API_KEY | codex login --with-api-key
```

#### IDE extension

On the signed-out screen, select **Use API Key**, enter your key, then select **OK**.

OpenAI bills API key usage through your OpenAI Platform account at standard API rates.

API key authentication supports local Codex workflows, but some features that rely on ChatGPT workspace access or cloud services are limited or unavailable. When you sign in with an API key, Codex uses standard API pricing instead of included ChatGPT plan credits.

Use API key authentication for programmatic Codex CLI workflows, such as CI/CD jobs. Don't expose Codex execution in untrusted or public environments.

### Check authentication or sign out

Open the profile menu to confirm the active account and workspace. To end the ChatGPT web session in that browser, select **Log out**.

Open the profile menu to see the active account or API key status. Select **Log out** to clear the current credentials.

Run `codex login status` to see the active authentication method. Run `codex logout` to clear the current credentials.

### Use Codex access tokens for enterprise automation

In ChatGPT Enterprise workspaces, admins can grant the access token permission so permitted members can create Codex access tokens for trusted, non-interactive Codex local workflows. Use an access token when automation needs ChatGPT workspace access, ChatGPT-managed Codex entitlements, or enterprise workspace controls without a browser sign-in.

Access tokens are intended for trusted scripts, schedulers, and private CI runners. For general OpenAI API calls, continue to use Platform API keys.

If your environment already provides a Codex access token, pipe it to the CLI:

```
printenv CODEX_ACCESS_TOKEN | codex login --with-access-token
```

### Secure your Codex cloud account

Codex cloud interacts directly with your codebase, so it needs stronger security than many other ChatGPT features. Enable multi-factor authentication (MFA).

If you use a social login provider (Google, Microsoft, Apple), you aren't required to enable MFA on your ChatGPT account, but you can set it up with your social login provider.

If you log in using an email and password, you must set up MFA on your account before accessing Codex cloud.

If your account supports more than one login method and one of them is email and password, you must set up MFA before accessing Codex, even if you sign in another way.

### Login caching

When you sign in to the ChatGPT desktop app, Codex CLI, or IDE extension using either ChatGPT or an API key, your login details are cached and reused. The CLI and extension share the same cached login details. If you log out from either one, you'll need to sign in again the next time you start the CLI or extension.

Codex caches login details locally in a plaintext file at `~/.codex/auth.json` or in your OS-specific credential store.

For sign in with ChatGPT sessions, Codex refreshes tokens automatically during use before they expire, so active sessions usually continue without requiring another browser login.

### Credential storage

Use `cli_auth_credentials_store` to control where the Codex CLI stores cached credentials:

```
# file | keyring | auto
cli_auth_credentials_store = "keyring"
```

- `file` stores credentials in `auth.json` under `CODEX_HOME` (defaults to `~/.codex`).
- `keyring` stores credentials in your operating system credential store.
- `auto` uses the OS credential store when available, otherwise falls back to `auth.json`.

If you use file-based storage, treat `~/.codex/auth.json` like a password: it contains access tokens. Don't commit it, paste it into tickets, or share it in chat.

### Enforce a login method or workspace

In managed environments, admins may restrict how users are allowed to authenticate:

```
# Only allow ChatGPT login or only allow API key login.
forced_login_method = "chatgpt" # or "api"

# When using ChatGPT login, restrict users to a specific workspace.
forced_chatgpt_workspace_id = "00000000-0000-0000-0000-000000000000"
```

If the active credentials don't match the configured restrictions, Codex logs the user out and exits.

### Login diagnostics

Direct `codex login` runs write a dedicated `codex-login.log` file under your configured log directory. Use it when you need to debug browser-login or device-code failures, or when support asks for login-specific logs.

### Custom CA bundles

If your network uses a corporate TLS proxy or private root CA, set `CODEX_CA_CERTIFICATE` to a PEM bundle before logging in. When `CODEX_CA_CERTIFICATE` is unset, Codex falls back to `SSL_CERT_FILE`. The same custom CA settings apply to login, normal HTTPS requests, and secure WebSocket connections.

```
export CODEX_CA_CERTIFICATE=/path/to/corporate-root-ca.pem
codex login
```

### Login on headless devices

If you are signing in to ChatGPT with the Codex CLI, there are some situations where the browser-based login UI may not work:

- You're running the CLI in a remote or headless environment.
- Your local networking configuration blocks the localhost callback Codex uses to return the OAuth token to the CLI after you sign in.

In these situations, prefer device code authentication (beta).

#### Preferred: Device code authentication (beta)

1. Enable device code login in your ChatGPT security settings (personal account) or ChatGPT workspace permissions (workspace admin).
2. In the terminal where you're running Codex, choose one of these options:
   - In the interactive login UI, select **Sign in with Device Code**.
   - Run `codex login --device-auth`.
3. Open the link in your browser, sign in, then enter the one-time code.

#### Fallback: Authenticate locally and copy your auth cache

If you can complete the login flow on a machine with a browser, you can copy your cached credentials to the headless machine.

1. On a machine where you can use the browser-based login flow, run `codex login`.
2. Confirm the login cache exists at `~/.codex/auth.json`.
3. Copy `~/.codex/auth.json` to `~/.codex/auth.json` on the headless machine.

Treat `~/.codex/auth.json` like a password: it contains access tokens. Don't commit it, paste it into tickets, or share it in chat.

Copy to a remote machine over SSH:

```
ssh user@remote 'mkdir -p ~/.codex'
scp ~/.codex/auth.json user@remote:~/.codex/auth.json
```

Or use a one-liner that avoids `scp`:

```
ssh user@remote 'mkdir -p ~/.codex && cat > ~/.codex/auth.json' < ~/.codex/auth.json
```

Copy into a Docker container:

```
# Replace MY_CONTAINER with the name or ID of your container.
CONTAINER_HOME=$(docker exec MY_CONTAINER printenv HOME)
docker exec MY_CONTAINER mkdir -p "$CONTAINER_HOME/.codex"
docker cp ~/.codex/auth.json MY_CONTAINER:"$CONTAINER_HOME/.codex/auth.json"
```

#### Fallback: Forward the localhost callback over SSH

If you can forward ports between your local machine and the remote host, you can use the standard browser-based flow by tunneling Codex's local callback server (default `localhost:1455`).

1. From your local machine, start port forwarding:

```
ssh -L 1455:localhost:1455 user@remote
```

2. In that SSH session, run `codex login` and follow the printed address on your local machine.

### Alternative model providers

When you define a custom model provider in your configuration file, you can choose one of these authentication methods:

- **OpenAI authentication**: Set `requires_openai_auth = true` to use OpenAI authentication. You can then sign in with ChatGPT or an API key. When `requires_openai_auth = true`, Codex ignores `env_key`.
- **Environment variable authentication**: Set `env_key = "<ENV_VARIABLE_NAME>"` to use a provider-specific API key from the local environment variable named `<ENV_VARIABLE_NAME>`.
- **No authentication**: If you don't set `requires_openai_auth` (or set it to `false`) and you don't set `env_key`, Codex assumes the provider doesn't require authentication. This is useful for local models.

---

## Access tokens

Create and manage access tokens for Codex programmatic workflows

### Overview

Codex access tokens are ChatGPT workspace credentials scoped to Codex permissions. They authenticate trusted non-interactive local workflows, including Codex CLI and app-server-based automation, with a ChatGPT workspace identity. Use them when a script, scheduled job, or CI runner needs repeatable local access.

Codex access tokens are currently supported for ChatGPT Business and Enterprise workspaces.

Access tokens are created in the ChatGPT admin console at [Access tokens](https://chatgpt.com/admin/access-tokens). They're tied to the ChatGPT user who creates them and that user's workspace. The tokens act as agent identities for programmatic local workflows.

If a Platform API key works for your automation, keep using API key auth. Use Codex access tokens when a trusted local workflow specifically needs ChatGPT workspace access, workspace managed entitlements, or enterprise controls.

Need to trigger a published ChatGPT workspace agent from your own system? Use a Workspace Agent access token for the Workspace Agents API instead. Codex access tokens authenticate trusted local workflows through Codex CLI or an app-server client; they do not authenticate workspace agent trigger calls. See [Authenticate with Workspace Agent access tokens](/workspace-agents/authentication).

### How access tokens work

Use an access token when Codex CLI or an app-server client needs to run without a user completing a browser sign-in. The token represents the ChatGPT workspace user who created it, so runs can use that user's access and appear in workspace governance data.

The client checks the token when a run starts and ties the run to that workspace identity. Treat the token like any other automation secret: store it in a secret manager, keep it out of logs, and rotate it regularly.

#### Use cases for access tokens:

- `codex exec` jobs that run from trusted automation.
- Local scripts that need repeatable, non-interactive Codex CLI runs.
- Trusted app-server-based automation.
- Enterprise workflows where usage should be associated with a ChatGPT workspace user instead of an API organization key.

#### Main risks to avoid:

- **Leaked secrets:** anyone with the token can start local runs through Codex CLI or an app-server client as the token creator. Store tokens in a secret manager, keep them out of logs, and rotate them regularly.
- **Runner trust:** public CI, forked pull requests, or shared machines can expose tokens to people outside your workspace. Use access tokens only on trusted runners.
- **Shared identities:** one person's token reused across unrelated teams makes ownership and audit trails harder to interpret. Create tokens for a specific workflow owner.
- **Stale credentials:** long-lived tokens can remain active after the workflow changes. Prefer time-limited tokens and revoke tokens that are no longer used.
- **Wrong credential type:** Codex access tokens are for trusted local automation through Codex CLI or an app-server client. Use Workspace Agent access tokens to trigger published ChatGPT workspace agents, and use Platform API keys for general OpenAI API calls.

### Enable access token creation

Use the access token permission in workspace settings to turn on access token creation for allowed members.

The access token permission controls token creation. It doesn't grant access to the ChatGPT desktop app, Codex CLI, or IDE extension, and it doesn't change a member's seat type, built-in workspace role, or local runtime permission profile. Configure those controls as needed.

For the relationship between these controls, see [Roles and workspace permissions](/codex/enterprise/roles-and-workspace-permissions).

1. Go to [Workspace Settings > Permissions & roles](https://chatgpt.com/admin/settings).
2. In the **Access tokens** section, turn on **Allow users to create access tokens** if all allowed members should be able to create access tokens.
3. If the workflow also needs a covered local surface, make sure **Allow members to use Codex Local** is turned on in the **Codex Local** section. This control covers local use in the ChatGPT desktop app, Codex CLI, and IDE extension.

Keep access token creation limited to people or service owners who understand where the token will be stored, which automation will use it, and how it will be rotated.

### Set an access token expiration limit

Workspace owners and admins can set the longest expiration that members can choose when they create a Codex access token. Go to [Workspace Settings > Permissions & roles](https://chatgpt.com/admin/settings), then set **Access token expiration limit** in the **Codex Local** section.

The limit applies to new access tokens. Existing tokens keep their current expiration.

### Create an access token

Use the Access tokens page to name the token and choose when it expires.

1. Go to [Access tokens](https://chatgpt.com/admin/access-tokens).
2. Select **Create**.
3. Enter a descriptive name, such as `release-ci` or `nightly-docs-check`.
4. Choose an expiration. Prefer a finite expiration such as 7, 30, 60, or 90 days. If you choose **No expiration**, rotate the token on a regular schedule.
5. Select **Create**.
6. Copy the generated access token immediately. You can't view it again after you close the modal.
7. Store the token in your secret manager or CI secret store.

The shortest custom expiration is one day. Revoked and expired tokens can't be used to start new authenticated runs.

### Use an access token with Codex CLI

For ephemeral automation, store the token in `CODEX_ACCESS_TOKEN` and run Codex CLI normally:

```
export CODEX_ACCESS_TOKEN="<access-token>"
codex exec --json "review this repository and summarize the top risks"
```

For a persistent local login, pipe the token to `codex login --with-access-token`:

```
printf '%s' "$CODEX_ACCESS_TOKEN" | codex login --with-access-token
codex exec "summarize the last release diff"
```

`codex login --with-access-token` stores an agent identity credential in Codex CLI auth storage. If you prefer not to persist credentials on the machine, use the `CODEX_ACCESS_TOKEN` environment variable instead.

`codex app-server` can use the same credential through `CODEX_ACCESS_TOKEN` or a login created with `codex login --with-access-token` to authenticate its OpenAI requests. That credential is separate from client-to-app-server transport authentication. For a remote WebSocket connection, configure a separate bearer or capability token as described in [App server](/codex/app-server); don't reuse the Codex access token as the transport token. See [Authentication and network environment variables](/codex/config-file/environment-variables#authentication-and-network).

### Rotate or revoke a token

Rotate access tokens the same way you rotate other automation secrets:

1. Create a replacement token.
2. Update the secret in the runner, scheduler, or secret manager.
3. Run a smoke test with the new token.
4. Revoke the old token from [Access tokens](https://chatgpt.com/admin/access-tokens).

From the Access tokens page, workspace owners and admins can revoke any workspace token. Members with access token permission can revoke only the tokens they created.

### Permission model

The workspace access token permission controls token creation. The **Allow members to use Codex Local** workspace permission separately gates access to local use in the ChatGPT desktop app, Codex CLI, and IDE extension. A member can have that local access without permission to create access tokens.

| Capability | Workspace owners and admins | Member with access token permission | Member without access token permission |
|---|---|---|---|
| Open [Access tokens](https://chatgpt.com/admin/access-tokens) | Yes | Yes | No |
| Create access tokens | Yes, for their own ChatGPT workspace identity | Yes, for their own ChatGPT workspace identity | No |
| List access tokens | Workspace list, including who created each token | Only tokens they created | No |
| Revoke access tokens from the Access tokens page | Any token in the workspace | Only tokens they created | No page access |
| Grant or remove access token permission | Yes | No | No |
| Manage other local-client or Codex cloud settings | Yes, based on workspace admin permissions | No, unless separately granted | No |

In short: workspace owners and admins manage access at the workspace level. Members need the access token permission to create and manage their own tokens, but that permission grants neither admin rights nor access to other members' tokens.

### Troubleshooting

#### The access tokens page returns 404 or forbidden

Ask a workspace owner or admin to confirm that your role includes **Allow users to create access tokens**. If your workflow also needs a covered local surface, confirm that **Allow members to use Codex Local** is enabled for local use in the ChatGPT desktop app, Codex CLI, and IDE extension.

#### `codex login --with-access-token` fails

Confirm that you copied the generated access token, not a browser session token or Platform API key. Also confirm that the token hasn't expired or been revoked.

### Related docs

- [Authentication](/codex/auth)
- [Non-interactive mode](/codex/non-interactive-mode)
- [Admin rollout guide](/codex/enterprise/admin-setup)
- [Groups and provisioning](/codex/enterprise/groups-and-provisioning)
- [Roles and workspace permissions](/codex/enterprise/roles-and-workspace-permissions)
- [Governance](/codex/enterprise/governance)

---

## Groups and provisioning

Understand group membership sources and their workspace access boundary

Groups organize ChatGPT workspace access for a set of members and can carry custom roles. Group membership is separate from local runtime policy and permissions in connected systems.

For the complete control model, see [Roles and workspace permissions](/codex/enterprise/roles-and-workspace-permissions).

### Compare membership sources

Each group has one authoritative membership source:

| Group type | Membership source | When it applies |
|---|---|---|
| Manually managed | ChatGPT workspace administration | The group is small, temporary, or not managed through directory sync |
| Identity-provider managed | Your identity provider through SCIM | Membership should follow the organization's directory and member-removal process |

Manual and identity-provider-managed groups can coexist. For synchronized groups, the identity provider is the membership source; later provisioning updates can overwrite workspace-side changes. The Help Center owns current SCIM behavior, supported attributes, and setup steps.

### Understand the access boundary

SCIM provisions workspace membership and group assignments. It doesn't grant permissions in GitHub, Google Drive, Slack, or another connected system. It also doesn't replace local runtime requirements or Platform API organization access.

Workspace RBAC and local runtime requirements are separate control systems. A group can be relevant to both, but don't infer a managed-requirements matching or precedence rule from workspace group order. Use [Managed configuration](/codex/enterprise/managed-configuration) for the documented delivery and local precedence rules.

### Use current setup procedures

Workspace administration details can change. Use these sources for current UI steps, availability, and limits:

- [Manage members, seat types, roles, and access](https://help.openai.com/en/articles/8266401-managing-members-seat-types-roles-and-access-in-chatgpt-enterprise)
- [Manage groups](https://help.openai.com/en/articles/9083985-group-permissions-in-gpts)
- [SCIM integration FAQ](https://help.openai.com/en/articles/10011769-openai-platform-scim-integration-faq)
- [Manage workspace settings](https://help.openai.com/en/articles/8411955)

### Related docs

- [Authentication](/codex/auth)
- [Roles and workspace permissions](/codex/enterprise/roles-and-workspace-permissions)
- [Managed configuration](/codex/enterprise/managed-configuration)
- [Admin rollout guide](/codex/enterprise/admin-setup)

---

## Roles and workspace permissions

Separate ChatGPT workspace access from local runtime, API, plugin, and source-system controls

Administration spans six control boundaries. Granting access at one boundary doesn't grant access at another. Use this page as the canonical map, then follow the linked source for current settings and procedures.

In workspace settings, **Codex Local** is a grouping label for certain local access and access-token controls, not a separate product or client. Individual controls in the group can have different scopes. The current **Allow members to use Codex Local** workspace permission covers local use in the ChatGPT desktop app, Codex CLI, and IDE extension. Managed configuration is a separate layer that constrains supported runtime behavior for covered capabilities in those clients. Features and effective requirements can differ by client and version.

### Understand the control boundaries

| Boundary | What it controls | What it doesn't control | Current source |
|----------|------------------|------------------------|-----------------|
| ChatGPT workspace | Membership, seats, built-in administration roles, and role-based access to supported workspace features | Local agent permissions, Platform API organization access, or permissions in a connected service | [ChatGPT workspace access](https://help.openai.com/en/articles/8266401-managing-members-seat-types-roles-and-access-in-chatgpt-enterprise) and [RBAC](https://help.openai.com/en/articles/11750701-rbac) |
| Local clients | Runtime behavior for covered capabilities in the ChatGPT desktop app, Codex CLI, and IDE extension, including approvals, filesystem and network access, permission profiles, and allowed integrations | A ChatGPT seat, feature or model entitlement, or access to external data | [Managed configuration](/codex/enterprise/managed-configuration) and [Permissions](/codex/permissions) |
| Codex cloud | Eligibility to use hosted Codex workflows and the cloud environments made available to the user | Local runtime policy or the repository permissions granted by a source system | [Cloud environments](/codex/environments/cloud-environment) |
| Platform API | Organization and project membership, API keys, model access, usage, and billing for API-authenticated work | ChatGPT workspace membership, local-client access, or Codex cloud access | [OpenAI API Platform](https://platform.openai.com/docs/overview) |
| Plugins and apps | Plugin availability and installation, bundled skills, app access, and supported app actions | Authorization in the connected service or broader local and cloud runtime permissions | [Plugin controls](/codex/enterprise/apps-and-connectors) |
| Connected systems | Which repositories, files, messages, and actions the authenticated account can access in the source system | ChatGPT workspace, plugin, Codex cloud, or Platform API entitlement | The connected service's administration and access controls |

A request must pass every boundary that applies to it. For example, workspace access can make an app available, but the connected service still decides which data the signed-in account can read. A local permission profile can restrict a run in a supported local client, but it can't grant a workspace feature or model.

### Assign workspace access

ChatGPT workspace administration separates product access from administrative authority. The workspace plan and a member's seat determine which product surfaces are available. Built-in workspace roles determine who can administer the workspace. Role-based access control (RBAC) determines which supported features members can use.

Administrators can assign custom roles through groups, and a member can receive access from more than one group. Because available seats, roles, and permissions change with product and plan updates, use the Help Center for the current permission list and setup procedure:

- [Manage members, seat types, roles, and access](https://help.openai.com/en/articles/8266401-managing-members-seat-types-roles-and-access-in-chatgpt-enterprise)
- [Configure role-based access control](https://help.openai.com/en/articles/11750701-rbac)
- [Manage groups](https://help.openai.com/en/articles/9083985-group-permissions-in-gpts)

### Apply local runtime policy

Local runtime policy constrains covered capabilities in the ChatGPT desktop app, Codex CLI, and IDE extension. Cloud-managed requirements additionally depend on supported ChatGPT sign-in and plan eligibility. Permission profiles and managed requirements can constrain commands, filesystem access, network access, approvals, and other local runtime behavior. They don't change the user's seat, workspace role, model entitlement, or permissions in an external system.

Users can select a built-in or custom permission profile when local policy allows it. Administrators can distribute defaults and requirements through the supported managed-configuration channels. See [Permissions](/codex/permissions) for profile behavior and [Managed configuration](/codex/enterprise/managed-configuration) for requirements, delivery, and precedence.

### Related docs

- [Admin rollout guide](/codex/enterprise/admin-setup)
- [Groups and provisioning](/codex/enterprise/groups-and-provisioning)
- [Workspace model availability](/codex/enterprise/workspace-model-availability)
- [Access tokens](/codex/enterprise/access-tokens)
- [Managed configuration](/codex/enterprise/managed-configuration)
- [Authentication](/codex/auth)

---

## Managed Configuration

Enforce runtime requirements across supported local clients and distribute managed defaults

### Overview

Managed configuration controls supported local runtime behavior for covered capabilities in the ChatGPT desktop app, Codex CLI, and IDE extension. Supported requirements can differ by client and version. Managed configuration doesn't grant ChatGPT workspace access, assign seats, or replace workspace role-based access control (RBAC). Use Roles and workspace permissions for workspace feature access and this page for local runtime policy.

Enterprise admins can control supported local client behavior in two ways:

- **Requirements**: admin-enforced constraints that users can't override.
- **Managed defaults**: starting values applied when a supported client launches. Users can still change settings during a run; the client reapplies managed defaults the next time it starts.

### Admin-Enforced Requirements (requirements.toml)

Requirements constrain security-sensitive settings including approval policy, approvals reviewer, automatic review policy, sandbox mode, permission profiles, web search mode, managed hooks, which MCP servers users can enable, and which user-configured plugin marketplace sources they can add, install from, or refresh. When resolving configuration, if a value conflicts with an enforced rule, the local client falls back to a compatible value and notifies the user. If you configure an `mcp_servers` allowlist, the client enables an MCP server only when both its name and identity match an approved entry; otherwise, the client disables it.

Requirements can also constrain feature flags via the `[features]` table in `requirements.toml`. Omitted keys remain unconstrained.

For Codex 0.138.0 or later, prefer permission profiles with `allowed_permission_profiles` and managed `default_permissions`. Use `allowed_sandbox_modes` only for legacy deployments that still configure `sandbox_mode`.

#### Locations and Precedence

Each supported local client composes requirements from lower to higher precedence:

1. System `requirements.toml` (`/etc/codex/requirements.toml` on Unix systems, including Linux and macOS, or `%ProgramData%\OpenAI\Codex\requirements.toml` on Windows).
2. Enterprise-managed requirements delivered in the cloud config bundle.
3. Legacy `managed_config.toml` fields that the local client reinterprets as requirements.
4. macOS managed preferences (MDM) delivered through `com.openai.codex:requirements_toml_base64`.

Higher-precedence layers override ordinary scalar and list values from lower layers. Tables merge by key, while requirements such as rules, hooks, and filesystem restrictions have field-specific composition behavior.

For backward compatibility, supported local clients reinterpret the legacy `approval_policy`, `approvals_reviewer`, and `sandbox_mode` fields as requirements.

#### Cloud-Managed Requirements

When a user signs in with ChatGPT on a supported plan, supported local clients can receive admin-enforced requirements associated with the workspace. This is a delivery channel for `requirements.toml`-compatible policy. It doesn't grant workspace access or replace workspace RBAC.

Open Managed configuration to create and assign cloud-managed requirements. Example policy requiring United States data residency:

```
enforce_residency = "us"
allowed_approval_policies = ["on-request"]
allowed_sandbox_modes = ["read-only", "workspace-write"]

[rules]
prefix_rules = [
  { pattern = [{ any_of = ["bash", "sh", "zsh"] }], decision = "prompt", justification = "Require explicit approval for shell entry points" },
]
```

Confirm that every managed client version supports the keys you select, and test the policy with a small group before an organization-wide assignment.

##### How Local Clients Apply Cloud-Managed Requirements

When a user starts a supported local client and signs in with ChatGPT on a supported plan, the client first checks for a valid, identity-matched cache entry. If no valid entry is available, the client fetches the applicable bundle with retries and writes a signed cache entry on success. If the request fails or times out and no valid cache is available, the cloud config bundle load returns an error rather than silently starting without the cloud-managed requirements layer.

After cache resolution, the client composes the cloud requirements with the other requirements layers. A background refresh can update the cache for a later start; it doesn't replace the requirements already loaded into the current process.

#### Example requirements.toml

This example blocks `--ask-for-approval never` and `--sandbox danger-full-access` (including `--yolo`):

```
allowed_approval_policies = ["untrusted", "on-request"]
allowed_sandbox_modes = ["read-only", "workspace-write"]
```

#### Disable Appshots

To disable Appshots for managed users, set the top-level `allow_appshots` requirement:

```
allow_appshots = false
```

Where Appshots are available, `allow_appshots = false` disables them. If you omit the key, requirements don't constrain Appshots, and normal product availability checks apply.

#### Disable Device Remote Control

To disable device remote control for managed users, set the top-level `allow_remote_control` requirement:

```
allow_remote_control = false
```

Where device remote control is supported, `allow_remote_control = false` disables it. If you omit the key, requirements don't constrain device remote control, and normal product availability checks apply. This requirement doesn't disable SSH remote connections.

#### Control Available Permission Profiles

Use `allowed_permission_profiles` to control which built-in and custom permission profiles users can select. This is the permission-profile counterpart to `allowed_sandbox_modes`; use the allowlist that matches how your users select permissions.

Permission-profile allowlists require Codex 0.138.0 or later. Codex 0.137.0 and earlier ignore `allowed_permission_profiles` and managed `default_permissions`.

Use the permission-profile examples below only after every managed client runs a supporting release. Don't deploy managed custom profiles until the fleet upgrade is complete.

When present, the table is the complete list of allowed profiles. It allows profiles set to `true` and denies profiles omitted or set to `false`, including built-ins added in future Codex versions.

##### Allow the Standard Profiles

This policy allows read-only and workspace access, but not full access:

```
default_permissions = ":workspace"

[allowed_permission_profiles]
":read-only" = true
":workspace" = true
# ":danger-full-access" is omitted, so it is denied.
```

##### Add a Managed Least-Privilege Default

Admins can define a custom profile in the same requirements source. Use organization-specific profile names that won't collide with names in users' loaded config. Custom names can't start with `:` or use the reserved `filesystem` name.

Don't deploy managed custom profiles to clients running Codex 0.137.0 or earlier.

Example:

```
default_permissions = "acme_review_only"

[allowed_permission_profiles]
":read-only" = true
":workspace" = true
acme_review_only = true
# ":danger-full-access" is intentionally omitted, so it is denied.

[permissions.acme_review_only]
description = "Review code without modifying the workspace."
extends = ":read-only"
```

##### Allow Only Enterprise-Defined Profiles

Omit all built-ins when users should select only admin-defined profiles:

```
default_permissions = "acme_workspace"

[allowed_permission_profiles]
acme_workspace = true

[permissions.acme_workspace]
description = "Workspace access with sensitive files denied."
extends = ":workspace"

[permissions.acme_workspace.filesystem]
glob_scan_max_depth = 3

[permissions.acme_workspace.filesystem.":workspace_roots"]
"**/*.env" = "deny"
```

The custom profile can extend `:workspace` even though users can't select the built-in `:workspace` profile directly.

##### Turn Off a Profile Allowed by Another Source

Permission allowlists combine by profile name. Because cloud requirements have higher precedence than system requirements, cloud requirements can use `false` to turn off a profile allowed by the system file.

Cloud requirements:

```
default_permissions = ":read-only"

[allowed_permission_profiles]
":read-only" = true
":workspace" = false
```

System requirements:

```
[allowed_permission_profiles]
":read-only" = true
":workspace" = true  # Not honored because cloud requirements set this to false.
```

Set `default_permissions` explicitly to an allowed profile. If it's omitted, the local runtime defaults to `:workspace` only when both `:workspace` and `:read-only` are explicitly allowed. When `allowed_permission_profiles` is absent, managed requirements don't restrict which profile names users can select. Every entry must name a built-in profile or a custom profile defined in a loaded config or requirements source. Define custom profiles in managed requirements to control their behavior centrally.

#### Override Sandbox Requirements by Host

Use `[[remote_sandbox_config]]` when one managed policy should apply different sandbox requirements on different hosts. For example, you can keep a stricter default for laptops while allowing workspace writes on matching dev boxes or CI runners. Host-specific entries currently override `allowed_sandbox_modes` only:

```
allowed_sandbox_modes = ["read-only"]

[[remote_sandbox_config]]
hostname_patterns = ["*.devbox.example.com", "runner-??.ci.example.com"]
allowed_sandbox_modes = ["read-only", "workspace-write"]
```

The local runtime compares each `hostname_patterns` entry against the best-effort resolved host name. It prefers the fully qualified domain name when available and falls back to the local host name. Matching is case-insensitive; `*` matches any sequence of characters, and `?` matches one character.

The first matching `[[remote_sandbox_config]]` entry wins within the same requirements source. If no entry matches, the local runtime keeps the top-level `allowed_sandbox_modes`. Host name matching is for policy selection only; don't treat it as authenticated device proof.

You can also constrain web search mode:

```
allowed_web_search_modes = ["cached"] # "disabled" remains implicitly allowed
```

`allowed_web_search_modes = []` allows only `"disabled"`. For example, `allowed_web_search_modes = ["cached"]` prevents live web search even in `danger-full-access` sessions.

#### Configure Network Access Requirements

`[experimental_network]` is experimental and may change. Do not enable these requirements broadly across an enterprise deployment without validating them on the local client versions and operating systems your users run. Windows support is still limited; avoid applying this policy to Windows users unless you have tested it in your environment.

Use `[experimental_network]` in `requirements.toml` when administrators should define network access requirements centrally. These requirements are separate from the user `features.network_proxy` toggle: they can configure sandbox networking without that feature flag, but they don't grant command network access when the active sandbox keeps networking off.

```
experimental_network.enabled = true
experimental_network.allowed_domains = [
  "api.openai.com",
  "*.example.com",
]
experimental_network.denied_domains = [
  "blocked.example.com",
  "*.exfil.example.com",
]
```

Use `experimental_network.managed_allowed_domains_only = true` only when you also define administrator-owned `allowed_domains` and want that allowlist to be exclusive. If it's `true` without managed allow rules, user-added domain allow rules don't remain effective.

The domain syntax, local/private destination rules, deny-over-allow behavior, and DNS rebinding limitations are the same as the sandbox networking behavior.

#### Pin Feature Flags

You can also pin feature flags for users receiving a managed `requirements.toml`:

```
[features]
personality = true
unified_exec = false

# Disable surface-specific features when needed.
browser_use = false
browser_use_full_cdp_access = false
browser_use_external = false
in_app_browser = false
computer_use = false
```

Use the canonical feature keys from `config.toml`'s `[features]` table for runtime features. The local runtime normalizes recognized features to meet these pins and rejects conflicting writes to `config.toml` or profile file feature settings.

- `in_app_browser = false` disables the built-in browser pane.
- `browser_use = false` disables Computer Use in browsers and Browser Agent availability.
- `browser_use_full_cdp_access = false` disables full CDP access in the local runtime, including Browser Developer mode, and prevents the ChatGPT desktop app from enabling the corresponding setting.
- `browser_use_external = false` disables external Browser Use.
- `computer_use = false` disables Computer Use, Record & Replay, and related install or setup flows.

If you omit these keys, policy allows the features, subject to normal client, platform, and rollout availability.

#### Restrict Locked Computer Use

To prevent Computer Use from operating after a managed Mac locks, add this requirement:

```
[computer_use]
allow_locked_computer_use = false
```

This requirement doesn't enable Computer Use. It only prevents locked use on macOS. If you omit it, requirements don't constrain locked use; normal product availability and the user's local setting still apply.

#### Configure Automatic Review Policy

Use `allowed_approvals_reviewers` to require or allow automatic review. Set it to `["auto_review"]` to require automatic review, or include `"user"` when users can choose manual approval.

Set `guardian_policy_config` to replace the tenant-specific section of the automatic review policy. The local runtime still uses the built-in reviewer template and output contract. Managed `guardian_policy_config` takes precedence over local `[auto_review].policy`.

```
allowed_approval_policies = ["on-request"]
allowed_approvals_reviewers = ["auto_review"]

guardian_policy_config = """
## Environment Profile
- Trusted internal destinations include github.com/my-org, artifacts.example.com,
  and internal CI systems.

## Tenant Risk Taxonomy and Allow/Deny Rules
- Treat uploads to unapproved third-party file-sharing services as high risk.
- Deny actions that expose credentials or private source code to untrusted
  destinations.
"""
```

#### Enforce Deny-Read Requirements

Admins can deny reads for exact paths or glob patterns with `[permissions.filesystem]`. Users can't weaken these requirements with local configuration.

```
[permissions.filesystem]
deny_read = [
  # values can be absolute paths...
  "/**/*.env",
  # ...or relative to $HOME/%USERPROFILE% using `~`.
  "~/.ssh",
  # But relative paths starting with `./` are not allowed.
]
```

When deny-read requirements are present, the local runtime rejects full-access permissions and keeps local execution in a read-only or workspace sandbox so it can enforce them. On native Windows, managed `deny_read` applies to direct file tools; shell subprocess reads don't use this sandbox rule.

#### Enforce Managed Hooks from Requirements

Admins can also define managed lifecycle hooks directly in `requirements.toml`. Use `[hooks]` for the hook configuration itself, and point `managed_dir` at the directory where your MDM or endpoint-management tooling installs the referenced scripts.

To enforce managed hooks even for users who turned hooks off locally, pin `[features].hooks = true` alongside `[hooks]`. To skip user, project, session, and plugin hooks while still allowing managed hooks, set `allow_managed_hooks_only = true`.

```
allow_managed_hooks_only = true

[features]
hooks = true

[hooks]
managed_dir = "/enterprise/hooks"
windows_managed_dir = 'C:\enterprise\hooks'

[[hooks.PreToolUse]]
matcher = "^Bash$"

[[hooks.PreToolUse.hooks]]
type = "command"
command = "python3 /enterprise/hooks/pre_tool_use_policy.py"
command_windows = 'py -3 C:\enterprise\hooks\pre_tool_use_policy.py'
timeout = 30
statusMessage = "Checking managed Bash command"
```

Notes:

- The local runtime enforces the hook configuration from `requirements.toml`, but it doesn't distribute the scripts in `managed_dir`.
- Deliver those scripts with your MDM or device-management solution.
- Managed hook commands should reference absolute script paths under the configured managed directory.
- `allow_managed_hooks_only = true` skips hooks from user, project, session, and plugin sources, but still loads hooks from `requirements.toml` and other managed config layers.

#### Enforce Command Rules from Requirements

Admins can also enforce restrictive command rules from `requirements.toml` using a `[rules]` table. These rules merge with regular `.rules` files, and the most restrictive decision still wins.

Unlike `.rules`, requirements rules must specify `decision`, and that decision must be `"prompt"` or `"forbidden"` (not `"allow"`).

```
[rules]
prefix_rules = [
  { pattern = [{ token = "rm" }], decision = "forbidden", justification = "Use git clean -fd instead." },
  { pattern = [{ token = "git" }, { any_of = ["push", "commit"] }], decision = "prompt", justification = "Require review before mutating history." },
]
```

To restrict which MCP servers a local client can enable, add an `mcp_servers` approved list. For stdio servers, match on `command`; for streamable HTTP servers, match on `url`:

```
[mcp_servers.docs]
identity = { command = "codex-mcp" }

[mcp_servers.remote]
identity = { url = "https://example.com/mcp" }
```

The string form of `identity.command` matches only the configured `command`. It doesn't inspect `args`, `cwd`, `env`, or `env_vars`.

To constrain a complete stdio invocation, match the executable and each positional argument:

```
[mcp_servers.internal.identity]
command = { executable = "/usr/local/bin/codex-mcp", args = [
  { match = "exact", value = "serve" },
  { match = "prefix", value = "--workspace=" },
] }
```

The executable, argument count, and argument order must match. Argument and URL rules support `exact`, `prefix`, and full-value `regex` matching. Structured command rules still don't inspect `cwd`, `env`, or `env_vars`. Plugin-bundled MCP servers use the same identity shapes under `plugins.<plugin>.mcp_servers.<server>`.

If `mcp_servers` is present but empty, the local client disables all MCP servers.

#### Restrict Plugin Marketplace Sources

To restrict operations on user-configured marketplace sources, set `restrict_to_allowed_sources = true` and define one or more source rules:

```
[marketplaces]
restrict_to_allowed_sources = true

[marketplaces.allowed_sources.company_plugins]
source = "git"
url = "https://github.com/example/company-plugins.git"
ref = "main"

[marketplaces.allowed_sources.internal_git]
source = "host_pattern"
host_pattern = '^git\.example\.com$'

[marketplaces.allowed_sources.local_plugins]
source = "local"
path = "/opt/company/codex-plugins"
```

Git rules match the normalized repository URL and, when present, an exact `ref`. Host patterns are regular expressions matched against the lowercase Git host; use `^` and `$` for a whole-host match. Local rules require an absolute, normalized path.

These requirements reject unmatched marketplace add, plugin install, and configured Git marketplace refresh operations for user-configured sources. Codex-managed OpenAI marketplaces remain available when their source and reserved name match. The requirements don't filter already configured user marketplaces or their plugins at runtime.

These source restrictions apply only where a local client supports plugin marketplace operations: ChatGPT Work and Codex in the ChatGPT desktop app, and Codex CLI. They don't add plugins to Chat, the IDE extension, or mobile.

### Managed Defaults (managed_config.toml)

Managed defaults merge on top of a user's local `config.toml` and take precedence over any CLI `--config` overrides, setting the starting values when a supported local client launches. Users can still change those settings during a run; the client reapplies managed defaults the next time it starts.

Make sure your managed defaults meet your requirements; the local runtime rejects disallowed values.

#### Precedence and Layering

The local runtime assembles the effective configuration in this order (top overrides bottom):

- Managed preferences (macOS MDM; highest precedence)
- `managed_config.toml` (system/managed file)
- `config.toml` (user's base configuration)

CLI `--config key=value` overrides apply to the base, but managed layers override them. This means each run starts from the managed defaults even if you provide local flags.

Cloud-managed requirements affect the requirements layer (not managed defaults).

#### Locations

- Linux/macOS (Unix): `/etc/codex/managed_config.toml`
- Windows/non-Unix: `~/.codex/managed_config.toml`

If the file is missing, the local runtime skips the managed layer.

#### macOS Managed Preferences (MDM)

On macOS, admins can push a device profile that provides base64-encoded TOML payloads at:

- Preference domain: `com.openai.codex`
- Keys:
  - `config_toml_base64` (managed defaults)
  - `requirements_toml_base64` (requirements)

The local runtime parses these "managed preferences" payloads as TOML. For managed defaults (`config_toml_base64`), managed preferences have the highest precedence. For requirements (`requirements_toml_base64`), precedence follows the cloud-managed requirements order. The same requirements-side `[features]` table works in `requirements_toml_base64`; use canonical feature keys there as well.

#### MDM Setup Workflow

The local runtime honors standard macOS MDM payloads, so you can distribute settings with tooling like `Jamf Pro`, `Fleet`, or `Kandji`. A lightweight deployment looks like:

1. Build the managed payload TOML and encode it with `base64` (no wrapping).
2. Drop the string into your MDM profile under the `com.openai.codex` domain at `config_toml_base64` (managed defaults) or `requirements_toml_base64` (requirements).
3. Push the profile, then ask users to restart the supported local client and confirm the startup config summary reflects the managed values.
4. When revoking or changing policy, update the managed payload; the client reads the refreshed preference the next time it launches.

Avoid embedding secrets or high-churn dynamic values in the payload. Treat the managed TOML like any other MDM setting under change control.

#### Example managed_config.toml

```
# Set conservative defaults
approval_policy = "on-request"
sandbox_mode    = "workspace-write"

[sandbox_workspace_write]
network_access = false             # keep network disabled unless explicitly allowed

[otel]
environment = "prod"
exporter = "otlp-http"            # point at your collector
log_user_prompt = false            # keep prompts redacted
# exporter details live under exporter tables; see Monitoring and telemetry above
```

#### Recommended Guardrails

- Prefer `workspace-write` with approvals for most users; reserve full access for controlled containers.
- Keep `network_access = false` unless your security review allows a collector or domains required by your workflows.
- Use managed configuration to pin OTel settings (exporter, environment), but keep `log_user_prompt = false` unless your policy explicitly allows storing prompt contents.
- Periodically audit diffs between local `config.toml` and managed policy to catch drift; managed layers should win over local flags and files.

---

## HIPAA Configuration Guide for Codex Local

### Overview

This guide helps organizations configure Codex Local (the app, IDE extension, and CLI) for workflows potentially involving protected health information (PHI). OpenAI supports HIPAA compliance through applicable Business Associate Agreements when using ChatGPT for Healthcare, ChatGPT for Clinicians, or Regulated workspaces.

### Key Responsibilities

**Shared Model**: OpenAI handles PHI received from Codex Local consistent with the BAA, while organizations bear responsibility for local workstation security, source repositories, local retention, MCP servers, Browser Use, Computer Use activity, and third-party service integrations.

**Your Obligations**: Organizations must secure local workstations, review HIPAA policies against final configurations, manage endpoint controls, and document data flows. This guidance is not legal advice.

### OpenAI's Security Framework

OpenAI maintains enterprise risk management with board-level governance, secure development practices including peer review and automated testing, vulnerability management programs, and comprehensive data protection controls including encryption in transit and at rest.

### Sign-In Configuration

Codex supports two authentication methods:

- **ChatGPT sign-in**: Follows workspace permissions and ChatGPT Enterprise settings
- **API key sign-in**: Follows OpenAI API organization settings (suitable for CI/CD workflows)

The BAA covers both methods when properly configured.

### Critical Configuration Controls

#### Core Settings Table

| Control | Setting | Purpose |
|---------|---------|---------|
| Sign-in | ChatGPT for workspace PHI; API keys for approved workflows | Determines governance authority |
| Workspace pinning | `forced_chatgpt_workspace_id` | Keeps PHI workflows in approved workspace |
| Approvals | `allowed_approval_policies = ["on-request", "untrusted"]` | Requires review of higher-risk actions |
| Permissions | `default_permissions = ":workspace"` | Restricts device access |
| Web search | `allowed_web_search_modes = ["cached"]` | Limits external data exposure |
| Features | Disable `computer_use`, `browser_use`, `in_app_browser` | Reduces PHI exposure to external apps |
| MCP servers | Empty by default; allowlist approved servers | Controls third-party tool access |
| History | `persistence = "none"` when required | Manages local transcript retention |

### Starter Configuration

Organizations receive a starter `requirements.toml` that establishes baseline controls:

```toml
allowed_approval_policies = ["on-request", "untrusted"]
allowed_approvals_reviewers = ["user"]
allowed_web_search_modes = ["cached"]
default_permissions = ":workspace"

[allowed_permission_profiles]
":read-only" = true
":workspace" = true

[features]
computer_use = false
browser_use = false
browser_use_full_cdp_access = false
in_app_browser = false

[mcp_servers]
# None allowed by default
```

### Configuration Examples

#### Google Drive App Integration

Organizations can enable Google Drive after confirming OAuth scopes, access controls, and third-party BAA status:

```toml
[features]
apps = true

[apps.google_drive]
enabled = true
destructive_enabled = false
default_tools_approval_mode = "prompt"

[apps.google_drive.tools."files/delete"]
enabled = false
```

#### Local GitHub Usage

For repository operations, implement review checkpoints:

```toml
[rules]
prefix_rules = [
  { pattern = [{ token = "git" }, { any_of = ["push", "commit"] }], 
    decision = "prompt", justification = "Require review before changing repository history." }
]
```

### Managed Configuration Hierarchy

Codex applies settings in this precedence order:

1. Cloud-managed requirements (highest priority)
2. macOS MDM requirements
3. System `requirements.toml`
4. User `config.toml` (lowest priority)

Managed defaults allow user changes within a session but reset at startup. Use requirements for non-bypassable compliance controls.

### Rollout Steps

1. Select ChatGPT, API key, or hybrid sign-in method
2. Confirm BAA coverage with OpenAI for chosen sign-in paths
3. Enable Codex Local and establish RBAC groups
4. Deploy admin-enforced `requirements.toml` and managed defaults
5. Train users on approval workflows and sandbox boundaries
6. Review third-party apps for PHI compatibility and BAA status
7. Monitor through Compliance API exports, workspace analytics, and audit logs

### Third-Party Service Considerations

Organizations remain responsible for confirming that third-party destinations (Google Drive, GitHub, Microsoft SharePoint, etc.) have appropriate BAAs before PHI exchange. OpenAI's BAA covers OpenAI's handling only, not third-party recipients.

### Key References

- Codex authentication and managed configuration documentation
- Agent approvals and security guidelines
- Codex Model Context Protocol (MCP) setup
- ChatGPT Healthcare and Regulated Workspace functionality
- Business Associate Agreement terms for API services

---

## Workspace model availability

Model availability depends on the product surface and authentication boundary. A ChatGPT workspace model setting isn't a universal model switch for Codex in the ChatGPT desktop app, Codex CLI, IDE extension, Codex cloud, or Platform API.

For the complete administration model, see [Roles and workspace permissions](/codex/enterprise/roles-and-workspace-permissions).

### Identify the model boundary

| Product or authentication boundary | Model access follows | Current source |
|---|---|---|
| ChatGPT workspace | The workspace plan, member access, workspace settings, and supported role permissions | [ChatGPT Enterprise and Edu models and limits](https://help.openai.com/en/articles/11165333-chatgpt-enterprise-models-limits) |
| Codex in the ChatGPT desktop app, Codex CLI, and IDE extension with ChatGPT sign-in | Models supported by the specific client and the access available to the signed-in ChatGPT identity | [Codex models](/codex/models) and current workspace guidance |
| Codex cloud | Models supported by hosted Codex workflows and the access available to the signed-in ChatGPT identity | [Codex models](/codex/models) and [Codex cloud](/codex/cloud) |
| Codex in the ChatGPT desktop app, Codex CLI, and IDE extension with API-key authentication | The OpenAI API organization and project associated with the key | [Authentication](/codex/auth) and the [OpenAI API Platform](https://platform.openai.com/docs/overview) |

Check the current source for the surface the user is actually using. Don't copy a model catalog or assume that a ChatGPT model-picker setting has the same effect for Codex in the ChatGPT desktop app, Codex CLI, IDE extension, Codex cloud, and the API Platform.

### Separate access from runtime permissions

Model access determines whether a model is available to the authenticated user on a supported surface. Local permission profiles and managed requirements determine what an agent can do after a local run starts, such as which files it can change or which network destinations it can reach.

A permission profile can't grant model access. Model access also can't weaken the sandbox, approval policy, network controls, or source-system permissions that apply to a run.

### Troubleshoot model access

If a user can't select an expected model:

- Confirm the product surface and sign-in method.
- Confirm the ChatGPT workspace or Platform API organization and project.
- Review the current access controls for that authentication boundary.
- Check whether the selected local client or Codex cloud supports the model.

### Current sources

- [ChatGPT Enterprise and Edu models and limits](https://help.openai.com/en/articles/11165333-chatgpt-enterprise-models-limits)
- [Manage workspace settings](https://help.openai.com/en/articles/8411955)
- [Role-based access control](https://help.openai.com/en/articles/11750701-rbac)
- [Codex models](/codex/models)
- [Codex feature availability by plan](/codex/pricing#feature-availability)
- [Authentication](/codex/auth)

### Related docs

- [Admin rollout guide](/codex/enterprise/admin-setup)
- [Groups and provisioning](/codex/enterprise/groups-and-provisioning)
- [Roles and workspace permissions](/codex/enterprise/roles-and-workspace-permissions)
- [Managed configuration](/codex/enterprise/managed-configuration)

---

## Plugin controls

Understand plugin availability, connector-backed capabilities, actions, and connected-system access

A plugin extends Codex by packaging skills and optional connectors so teams can distribute workflows and knowledge. Learn more about [plugins](/codex/plugins), [skills](/codex/skills-and-plugins), and [apps and connectors](https://help.openai.com/en/articles/11487775).

ChatGPT presents connectors as apps. If a plugin includes a connector-backed capability, workspace admins must enable the corresponding app and configure its access before members can use that part of the plugin.

Plugins are available in ChatGPT Work on the web, in ChatGPT Work and Codex in the ChatGPT desktop app, and through the Codex CLI plugin browser. Availability on those surfaces doesn't make plugins available in Chat, the IDE extension, or mobile.

For the complete administration model, see [Roles and workspace permissions](/codex/enterprise/roles-and-workspace-permissions).

### Understand the capability chain

Each layer has a separate scope and control surface:

| Layer | What it determines | Where to manage it |
|-------|-------------------|-------------------|
| Plugin availability and installation | Whether the plugin bundle is available to the user | [Workspace settings](https://chatgpt.com/admin/settings) for supported web and desktop surfaces; the CLI plugin browser for CLI |
| Bundled skills | Which reusable instructions the installed plugin contributes | The plugin package and [Skill controls](/codex/enterprise/skills) |
| Connector access (Apps) | Whether users can use a connector-backed capability | [Workspace apps](https://chatgpt.com/admin/ca) and [Permissions & roles](https://chatgpt.com/admin/settings) |
| Connector actions and app permissions | Which actions users can run and when ChatGPT asks before using the app | The app's Action control and App permissions in [Workspace apps](https://chatgpt.com/admin/ca) |
| Source-system authorization | Which external data and actions the authenticated identity can access | The connected service and its identity provider |
| Runtime permissions | What an agent can do after it receives data or a tool | The runtime, sandbox, and approval controls for the active surface |

Depending on the workflow, admins can govern plugin availability, connector access, app actions and permissions, provider authorization, and runtime policy independently.

### Plugin availability controls

Workspace plugin controls determine whether a plugin is available or installed for supported workspace roles. The Codex CLI plugin browser controls CLI installation through its own path. See [Build plugins](/codex/build-plugins) for packaging and distribution.

### Connector-backed capability controls

In ChatGPT, **Apps** is the product label for connected capabilities, including connectors that search, retrieve, sync, or act on external systems. A plugin can refer to an app so its workflows can use those connector capabilities.

Manage connector-backed capabilities from [Workspace apps](https://chatgpt.com/admin/ca) and [Permissions & roles](https://chatgpt.com/admin/settings). Available controls let admins:

- Enable reviewed apps and assign access by workspace role.
- For apps that support Action control, allow read-only actions or an approved custom set, including how the workspace handles newly added actions.
- Set app permissions that determine when ChatGPT asks before using an app.
- Keep access within the scopes and permissions granted by each connected service and authenticated user.

For current availability and procedures, see [Admin controls, security, and compliance in apps](https://help.openai.com/en/articles/11509118).

### Choose a starting set of apps

For a broad initial rollout, consider app categories teams use every day: email, calendar, and file or document systems such as Google Drive or Notion. Use the [ChatGPT app directory](https://chatgpt.com/apps) to confirm current availability and capabilities.

Start with read actions. Enable write actions only after reviewing the app's owner, requested scopes, data access, external effects, and recovery path.

### Understand data flow and security

When ChatGPT uses a connector-backed app, "the app sends a request to the connected service and returns data or action results allowed by the authenticated user's provider permissions." Custom apps expose these operations as tools through Model Context Protocol (MCP).

For non-synced app use, ChatGPT processes Chat and deep-research data transiently and doesn't index it. Apps with sync index selected connected content in advance. This indexing distinction doesn't replace normal conversation-retention controls; conversations that use apps remain available through the Compliance API.

OpenAI's current app guidance documents encryption in transit and at rest, per-user authorization, role and action controls, restricted network access for app conversations, and no model training on information accessed from apps for Business, Enterprise, and Edu customers. Review the connected service's scopes, retention, and data-residency policies because those policies apply when a request reaches that service.

### Use current procedures

- [Admin controls, security, and compliance in apps](https://help.openai.com/en/articles/11509118)
- [Apps in ChatGPT](https://help.openai.com/en/articles/11487775)
- [Apps with sync](https://help.openai.com/en/articles/10847137)
- [Manage workspace settings](https://help.openai.com/en/articles/8411955)
- [Plugins](/codex/plugins)
- [Skills and plugins](/codex/skills-and-plugins)
- [Build plugins](/codex/build-plugins)
- [Admin rollout guide](/codex/enterprise/admin-setup)

---

## Skill controls

Skills are reusable workflows made from instructions and supporting resources. ChatGPT workspace Skills, filesystem skills used by covered local capabilities in the ChatGPT desktop app, Codex CLI, or IDE extension, and plugins that package skills have separate lifecycle and access controls.

For the complete administration model, see Roles and workspace permissions.

### Distinguish the distribution models

| Distribution model | Use it for | Administration boundary |
|---|---|---|
| ChatGPT workspace Skill | Sharing or installing an approved workflow through supported ChatGPT workspace features | ChatGPT workspace skill permissions and lifecycle controls |
| Local filesystem skill | Loading an installed workflow from a repository, user, administrator, or bundled system location | Filesystem distribution, local client configuration, and runtime permissions |
| Plugin | Packaging one or more skills with optional apps, MCP servers, hooks, and presentation metadata | Plugin availability and installation, plus the separate controls for every bundled capability |

ChatGPT workspace skill distribution, local filesystem skill installation, and surface-specific plugin installation are separate paths. Moving a skill doesn't transfer ChatGPT workspace ownership, sharing, role assignments, plugin installation state, or app authorization.

Plugins are available in ChatGPT Work on the web, in ChatGPT Work and Codex in the ChatGPT desktop app, and through the Codex CLI plugin browser. They aren't available in Chat, the IDE extension, or mobile.

### Owning controls

See Build skills for filesystem locations and authoring, Skills in ChatGPT for current workspace procedures, and Build plugins for plugin packaging.

ChatGPT workspace controls don't install local filesystem skills or plugins. Filesystem distribution doesn't assign ChatGPT workspace ownership or roles. Plugin installation doesn't grant access to an app, MCP server, or connected service. Configure each capability through the control surface that owns it.

### Related docs

- Skills and plugins
- Plugins
- Build skills
- Build plugins
- Admin rollout guide
- Plugin controls

---

## Governance

Choose the appropriate analytics, usage, and audit surface for each administration question

Governance for Codex activity spans interactive analytics, programmatic reporting, related ChatGPT usage controls, and audit records. Choose the surface that matches the question; analytics and compliance data serve different purposes.

### If you need to

| Need | Start with |
|------|-----------|
| Understand adoption across ChatGPT | [Workspace analytics](/codex/enterprise/workspace-analytics) |
| Review Codex adoption and activity interactively | [Codex analytics](#analytics-dashboard) |
| Load aggregated Codex reporting into another system | [Analytics API](/codex/enterprise/analytics-api) |
| Export records for audit or investigation | [Compliance API](/codex/enterprise/compliance-api) |
| Review plan-dependent ChatGPT workspace credit controls | [ChatGPT usage limits and spend controls](/codex/enterprise/usage-limits) |

### Open the administration surfaces

- Open [Workspace analytics](https://chatgpt.com/admin/usage) for interactive workspace reporting. The [Workspace analytics guide](https://help.openai.com/en/articles/10875114-workspace-analytics-for-chatgpt-enterprise-and-edu) describes the current roles and views.
- Open the authenticated [Codex Analytics API reference](https://chatgpt.com/codex/cloud/settings/apireference) when you need scheduled, programmatic reporting.
- Open the authenticated [Admin API reference](https://chatgpt.com/admin/api-reference) and the [Compliance Platform guide](https://help.openai.com/en/articles/9261474-compliance-api-for-chatgpt-enterprise-edu-and-chatgpt-for-teachers) for audit and investigation integrations.

For example, use workspace analytics for a quick adoption check, the Analytics API to load aggregated Codex reporting into a business intelligence system, and the Compliance API to send auditable records to a SIEM or electronic discovery workflow.

### Analytics dashboard

ChatGPT provides workspace-wide analytics for broad adoption and engagement. Codex analytics focuses on Codex activity. Both are interactive reporting surfaces, not raw audit logs.

Use [Workspace analytics](/codex/enterprise/workspace-analytics) to compare the two experiences and find their current owner-maintained sources. You can also open [Workspace analytics](https://chatgpt.com/admin/usage) directly. Don't build a durable reporting contract from dashboard labels or downloaded report fields; those can change as the product evolves.

### Related ChatGPT usage controls

ChatGPT workspace usage controls are separate from analytics and don't configure feature entitlements. Depending on the plan, eligible Codex activity can consume ChatGPT workspace credits, and exhausted limits can pause access to eligible features. These controls don't set a universal Codex limit or govern Platform API billing.

See [ChatGPT usage limits and spend controls](/codex/enterprise/usage-limits) for the durable boundary and current Help Center sources.

### Analytics API

Use the Analytics API for programmatic, aggregated Codex reporting. It's appropriate for data warehouses, business intelligence systems, and internal reporting that shouldn't depend on an interactive dashboard.

The authenticated API reference owns access requirements, routes, schemas, fields, reporting windows, and pagination. See [Analytics API](/codex/enterprise/analytics-api) for the conceptual integration boundary and the canonical reference link.

### Compliance API

Use the Compliance API for security, legal, and governance workflows that need auditable records. It's not an adoption or productivity dashboard.

The authenticated API reference owns the current routes, event coverage, schemas, filters, retention behavior, permission requirements, and request mechanics. See [Compliance API](/codex/enterprise/compliance-api) for the conceptual integration boundary and the canonical reference link.

For rollout sequencing and verification across these surfaces, use the [Admin rollout guide](/codex/enterprise/admin-setup).

### Related docs

- [Admin rollout guide](/codex/enterprise/admin-setup)
- [Workspace analytics](/codex/enterprise/workspace-analytics)
- [Analytics API](/codex/enterprise/analytics-api)
- [Compliance API](/codex/enterprise/compliance-api)

---

## Workspace analytics

Compare ChatGPT workspace analytics, Codex analytics, reporting APIs, and audit records

Use ChatGPT workspace analytics for broad workspace adoption. Use Codex analytics for Codex-focused reporting. Use the Analytics API for programmatic aggregates and the Compliance API for auditable records.

These reporting surfaces don't grant product access or set runtime policy. See [Roles and workspace permissions](/codex/enterprise/roles-and-workspace-permissions) for the administration boundaries.

### Choose a reporting surface

| Surface | Use it for | Contract owner |
|---------|-----------|----------------|
| ChatGPT workspace analytics | Interactive, workspace-wide adoption and engagement reporting | [Workspace analytics Help Center guidance](https://help.openai.com/en/articles/10875114) |
| Codex analytics | Interactive reporting focused on Codex adoption and activity | The authenticated [Codex analytics dashboard](https://admin.openai.com/analytics/codex) |
| Analytics API | Programmatic, aggregated Codex reporting | The authenticated [Codex Analytics API reference](https://chatgpt.com/codex/cloud/settings/apireference) |
| Compliance API | Audit, security, legal, and investigation records | The authenticated [Admin API reference](https://chatgpt.com/admin/api-reference) |

### Review ChatGPT workspace analytics

ChatGPT workspace analytics provides an interactive view of adoption and engagement across supported workspace features. Availability, roles, dashboard sections, freshness, privacy behavior, and export formats can change. Use [Workspace analytics for ChatGPT Enterprise and Edu](https://help.openai.com/en/articles/10875114) for current coverage and procedures.

Treat downloaded reports as identifiable organizational data. Apply the organization's access, storage, and retention policy instead of assuming that an export has the same privacy characteristics as an aggregated dashboard.

### Review Codex analytics

The authenticated [Codex analytics dashboard](https://admin.openai.com/analytics/codex) focuses on Codex reporting. Use it for interactive exploration, not as a stable schema contract. Dashboard categories, fields, filters, and export formats can change independently of this page.

For automated reporting, use the [Analytics API](/codex/enterprise/analytics-api) and follow its authenticated reference. For auditable records, use the [Compliance API](/codex/enterprise/compliance-api).

### Interpret reporting data

Keep these boundaries in mind:

* ChatGPT workspace analytics and Codex analytics cover different product scopes.
* Aggregated analytics and audit records serve different purposes and have separate contracts.
* Analytics describes activity; it doesn't grant access or change runtime permissions.
* [ChatGPT usage limits and spend controls](/codex/enterprise/usage-limits) are a separate, plan-dependent workspace boundary.

---

## Analytics API

Understand the purpose and administration boundary of the Codex Analytics API

### Overview

The Codex Analytics API provides aggregated Codex usage and activity metrics for a ChatGPT workspace.

The authenticated Codex Analytics API reference is the source of truth for current access requirements, routes, request and response schemas, metrics, time semantics, and pagination.

### When to use the Analytics API

The Analytics API is appropriate when you need to:

- Automate recurring Codex reporting.
- Join aggregated Codex metrics with internal organizational data.
- Build a controlled reporting layer for approved audiences.
- Avoid coupling an integration to an interactive dashboard.

It's not a raw audit-log interface. Use the Compliance API when the workflow requires auditable activity records.

### Confirm the administration boundaries

Analytics API results are scoped to a ChatGPT workspace, but requests authenticate with a Platform organization API key. The key's organization must match the organization associated with the workspace.

The authenticated reference owns current key provisioning, scope requirements, routes, schemas, fields, time semantics, and pagination behavior. This page doesn't duplicate that contract.

### Related docs

- [Workspace analytics](/codex/enterprise/workspace-analytics)
- [Admin rollout guide](/codex/enterprise/admin-setup)
- [Governance](/codex/enterprise/governance)
- [Compliance API](/codex/enterprise/compliance-api)

---

## Compliance API and audit events

Use the Compliance API for security, legal, governance, and investigation workflows that require auditable records. Use analytics, not compliance records, to measure adoption and trends.

The authenticated Admin API reference is the source of truth for current access requirements, event coverage, routes, schemas, filters, retention, and request behavior.

For an overview of the available compliance surfaces and common integration patterns, see the Compliance Platform guide.

### When to use the Compliance API

The Compliance API is appropriate when you need to:

- Export supported records into an audit or investigation system.
- Apply organizational retention and legal-hold processes.
- Correlate Codex activity with other security or identity data.
- Support approved security, legal, or governance investigations.

It's not a productivity dashboard. Don't use it to infer code quality or individual performance. Use Workspace analytics or the Analytics API for adoption reporting.

### Get started

1. Open the Admin API reference and confirm that your administrator role can access the compliance resources you need.
2. Use the append-only compliance log stream for ongoing collection. Check the authenticated reference for the currently supported resources and retrieval patterns.
3. Test ingestion into a non-production security information and event management (SIEM) system or data lake.
4. Schedule continuous collection and apply your organization's access, retention, and legal-hold controls to exported records. Don't assume the source retention window replaces your organization's retention policy.

### Confirm the administration boundaries

Compliance coverage follows the ChatGPT workspace and the products represented in the current authenticated reference. Platform API organization data follows its own API data and administration controls.

The authenticated reference owns the current routes, event coverage, schemas, filters, retention behavior, permission requirements, and request mechanics.

### Related docs

- Workspace analytics
- Admin rollout guide
- Governance
- Analytics API

---

## Deploy the Windows app

Choose an enterprise installation and update path for the ChatGPT desktop app on Windows

Choose a deployment path based on who controls installation and updates and whether your network allows Microsoft's app-distribution services. The app is Store-signed, but users don't need to open the Microsoft Store. Standard installation and updates use Microsoft's app installation and Windows Update services.

### Deployment Options

| Scenario | Deployment path |
|----------|-----------------|
| Users install and update their own apps | Use the [web installer](https://get.microsoft.com/installer/download/9PLM9XGG6VKS?cid=website_cta_psi) |
| IT deploys the app and Microsoft update services are available | Deploy the Microsoft Store app through Intune or another management tool |
| IT controls update timing or blocks Microsoft update services | Redeploy each new version through your software-management process |
| Your network blocks Microsoft app-distribution services | Use the offline MSIX package for restricted networks |

### Let users install and update the app

If users can manage their own applications, direct them to the [web installer](https://get.microsoft.com/installer/download/9PLM9XGG6VKS?cid=website_cta_psi). The installer provides the standard installation and automatic-update experience. Microsoft Store components may appear during installation or updates, but users don't need to browse the Store themselves.

You can also install the app from the command line:

```bash
winget install --id 9PLM9XGG6VKS -s msstore
```

### Deploy with an enterprise management tool

If your organization centrally manages software, deploy the Microsoft Store app through Microsoft Intune or another compatible mobile device management (MDM) or software-deployment platform. Search for ChatGPT from OpenAI in the Microsoft Store app flow, or use this Store product ID:

```
9PLM9XGG6VKS
```

This is the recommended enterprise deployment method when your environment allows Microsoft Store app deployment and update endpoints. Users don't need to open the Microsoft Store themselves.

For Intune setup details, see [Add Microsoft Store apps to Microsoft Intune](https://learn.microsoft.com/en-us/intune/app-management/deployment/add-microsoft-store).

### Control update timing

If your organization blocks automatic app updates, Windows Update, or Microsoft Store update endpoints, your IT team owns the update lifecycle. Codex can run in this environment, but you must redeploy newer packages through your change management process.

Redeploy the package with each Codex release when possible. If that cadence isn't practical, check for a newer package at least weekly and redeploy when a newer version is available. Users should restart the app after you deploy an update.

Deferring updates is a security and reliability tradeoff because the app includes browser and runtime components that receive regular updates.

### Deploy without Microsoft distribution services

If your environment can't use the standard Microsoft distribution services, download [Codex-x64.zip](https://persistent.oaistatic.com/codex-app-prod/Codex-x64.zip) as an offline deployment option. The archive contains the Store-signed offline MSIX package and its associated license files. Ingest these files into your MDM or software-deployment platform.

The offline package:

- Supports deployment in restricted environments
- Doesn't provide a standalone MSI or non-Store EXE
- Doesn't let users update the app themselves
- Requires your organization to deploy newer packages when updating the app

### Related resources

- [ChatGPT desktop app for Windows](/codex/app/windows)

---

## Remote connections

Use Remote to access work on a connected computer, or connect the ChatGPT desktop app to projects over SSH

Remote access uses the connected host's projects, tasks, files, credentials, permissions, plugins, Computer Use, browser setup, and local tools.

### What you can do remotely

- Start new tasks in projects on the host, or continue existing ones.
- Send follow-up instructions, answer questions, and steer active work.
- Approve commands and other actions.
- Review outputs, diffs, test results, terminal output, and screenshots.
- Get notified when ChatGPT completes a task or needs your attention.
- Switch between connected hosts and tasks.

### Before you set up Remote

Remote supports hosts running the ChatGPT desktop app on macOS and Windows. You can control a host from ChatGPT on iOS or Android, or from another Mac or Windows device when **Control other devices** is available.

Make sure you have:

- Codex access in the ChatGPT account and workspace you want to use.
- The latest ChatGPT mobile app on an iOS or Android device.
- The latest ChatGPT desktop app for macOS or Windows running on a host that's awake, online, and signed in to the same account and workspace.
- Any required multi-factor authentication, SSO, or passkey configuration for that account or workspace.

If you use Codex through a ChatGPT workspace, your admin may need to enable Remote Control access before you can connect from your phone.

### Set up Remote

Start in the ChatGPT desktop app on the host you want to connect. The setup flow enables remote access for that host, then shows a QR code you can scan from your phone.

1. Start Remote setup.

   Open the app on the host and select **Set up Remote** in the sidebar.

2. Scan the QR code.

   Use your phone to scan the QR code shown by the app.

3. Finish setup in ChatGPT.

   ChatGPT opens the Remote setup flow. Confirm the same ChatGPT account and workspace, then complete any required multi-factor authentication, SSO, or passkey steps.

4. Review host settings.

   In the app on the host, use **Settings > Connections** to manage connected devices.

### Choose what to connect

#### Your laptop or desktop

Connect the Mac or Windows PC where the desktop app is already installed. This gives remote access to the same projects, tasks, credentials, plugins, and local setup you already use.

On a Mac laptop, remote access can stay available with the lid open and power connected. With the lid closed, connect an external display as well.

#### A dedicated always-on computer

Use a dedicated always-on Mac or Windows PC when you want ChatGPT to stay reachable for longer-running work.

#### A remote development environment

Use an SSH host or managed remote development environment when the project already lives in a remote environment.

### What comes from the connected host

Your phone sends prompts, approvals, and follow-up messages to ChatGPT. The connected host provides the environment ChatGPT uses.

That means:

- Repository files and local documents come from the connected host.
- Shell commands run on that host or remote environment.
- MCP servers, skills, browser access, and Computer Use come from that host's configuration.
- Signed-in websites and desktop apps are available only when the host can access them.

### Pick up work from another device

You can continue work from another signed-in device running the ChatGPT desktop app and supporting remote control.

### Connect to an SSH host

In the ChatGPT desktop app, add remote projects from an SSH host and run tasks against the remote filesystem and shell.

1. Add the host to your SSH config so Codex can auto-discover it.

```bash
Host devbox
  HostName devbox.example.com
  User you
  IdentityFile ~/.ssh/id_ed25519
```

2. Confirm you can SSH to the host from the machine running the app.

```bash
ssh devbox
```

3. Install and authenticate Codex on the remote host.

   The app starts the remote Codex app server through SSH, using the remote user's login shell.

4. In the app, open **Settings > Connections**, add or enable the SSH host, then choose a remote project folder.

### Hand off a task between hosts

Handoff moves an existing task and its Git state between your local computer and a connected remote host.

Before you hand off a task, connect the destination host and save a project for the same Git repository on that host.

To hand off a task:

1. Open the task in the desktop app.
2. In the task footer, select the current run location, then select the destination host.
3. Review the destination and branch, then select **Hand off**.

### Authentication and network exposure

Remote connections use SSH to start and manage the remote Codex app server. Don't expose app-server transports directly on a shared or public network.

If you need to reach a remote machine outside your current network, use a VPN or mesh networking tool instead of exposing the app server directly to the internet.

### Troubleshooting

#### You don't see the host on your phone

Confirm that the desktop app is running on the host, you've enabled **Allow other devices to connect**, and both devices use the same ChatGPT account and workspace.

#### Remote Control is off after you sign back in

Signing out of ChatGPT turns off **Remote Control**, but it doesn't remove your existing device pairings.

#### The approval request doesn't appear

In the ChatGPT mobile app, open **Remote**. If you use a ChatGPT workspace, ask your admin to confirm that they've enabled Remote Control access.

#### The remote session disconnects

Check whether the host went to sleep, lost network access, or closed the app. Keep the host awake and connected while ChatGPT works.

#### Authentication blocks setup

Complete the account or workspace authentication prompt shown during setup. If setup still fails, ask your workspace admin to confirm that they've enabled Remote Control access.

---

## Use Codex with Amazon Bedrock

Configure Codex to use OpenAI models available through Amazon Bedrock

Codex can run locally and send model requests to Bedrock using AWS-managed authentication and access controls.

### How it works

When configured with Amazon Bedrock as the model provider, Codex sends requests directly to Amazon Bedrock rather than through OpenAI-hosted services. Bedrock provides an OpenAI-compatible Responses API implementation for supported OpenAI models. Users authenticate with Bedrock API keys or AWS IAM credentials instead of ChatGPT sign-in.

### Before you start

Required prerequisites:

- Access to supported OpenAI models in Amazon Bedrock
- An AWS Region where the selected model is available
- Authentication for the Amazon Bedrock Mantle path configured for the AWS account

### Configure Codex

Add the model provider to `~/.codex/config.toml`:

```toml
model_provider = "amazon-bedrock"
```

Note: This covers the Amazon Bedrock Mantle path in commercial AWS Regions only; GovCloud Regions are not supported.

### Authentication options

Codex checks authentication methods in this order:

#### Option 1: Bedrock API key

Requires specifying a Region:

```bash
export AWS_BEARER_TOKEN_BEDROCK=<your-bedrock-api-key>
export AWS_REGION=us-east-2
```

#### Option 2: AWS SDK credentials

Use standard AWS SDK credential sources:

1. Shared AWS config and credentials files via `aws configure`
2. Environment variables (AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_SESSION_TOKEN)
3. AWS Management Console credentials via `aws login`
4. AWS SSO or named profiles
5. Federated identity with `credential_process`

### Desktop app and VS Code extension

Since these may not inherit shell environment variables, place required values in `~/.codex/.env`:

```bash
export AWS_BEARER_TOKEN_BEDROCK=<your-bedrock-api-key>
export AWS_REGION=us-east-2
```

Then restart the application.

### Verify setup

- Check `/status` in Codex CLI to confirm `amazon-bedrock` provider is active
- Verify the model is available in your configured AWS Region
- Confirm the AWS identity has permission to access the selected model

### Supported models

Exact model IDs:

- `openai.gpt-5.5`
- `openai.gpt-5.4`

Model availability varies by region; consult AWS documentation for regional compatibility.

### Feature availability

This configuration supports local Codex workflows. Cloud-dependent features including Fast Mode (which requires priority processing) are unavailable. The detailed compatibility table indicates:

Available: Local code review, auto-review, sandboxing, scheduled tasks, Git tools, local environments, appshots, some browser/computer use features (with regional limits), custom instructions, skills, local plugins, MCP, and subagents.

Unavailable: Codex cloud, ChatGPT desktop for cloud tasks, image generation, voice dictation, web search, plugin sharing, connectors, memories, chronicle, sites, GitHub integration, Slack/Linear integration, enterprise SSO features, analytics, and Codex Security.

### Troubleshooting

Verify:

- Model ID exactly matches a supported model
- AWS Region supports the selected model
- Bedrock API key or AWS credentials are valid and not expired
- AWS identity has necessary permissions
- No expired tokens in `AWS_BEARER_TOKEN_BEDROCK`
- Desktop app/VS Code users have environment variables in `~/.codex/.env`

### Support boundaries

OpenAI Support assists with Codex client setup and local behavior. For AWS credentials, IAM permissions, Bedrock access, billing, or service issues, contact your AWS administrator or AWS Support.
