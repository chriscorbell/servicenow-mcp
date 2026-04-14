# ServiceNow Workspace Instructions

This workspace exists to assist the user with ServiceNow work, including:

- Troubleshooting issues in ServiceNow
- Investigating behavior, data, configuration, and platform problems
- Developing fixes and enhancements in ServiceNow
- Comparing behavior across configured ServiceNow environments
- Supporting implementation, validation, and documentation of ServiceNow changes

## MCP Usage

When ServiceNow data or configuration needs to be inspected, use the configured ServiceNow MCP server(s).

When interacting with ServiceNow through MCP:
- Use the `dev` instance to build and refine fixes, enhancements, and new features
- Use `qa` to test and validate changes promoted from `dev`. Do not treat `qa` as the primary development environment
- After `qa` validation, promote changes to `uat` via update set. Use `uat` only for acceptance testing performed by human end-users, not for new development or routine technical validation
- Do not promote anything from `uat` to `prod` via update set until the user confirms `uat` testing is complete and explicitly approves the production release
- Treat `prod` as read-only at all times. Never perform write operations in `prod` under any circumstances
- Prefer direct MCP queries to inspect records, tables, flows, catalog items, scripts, configuration, and related metadata
- Compare instances when diagnosing environment differences, regressions, configuration drift, or deployment issues

## Working Style

- Be practical, thorough, and evidence-driven
- Treat these instances as business-critical. Mistakes can impact a large enterprise with over 100,000 users and disrupt operations
- Gather facts from the configured ServiceNow instances before concluding root cause
- Explain findings clearly and tie recommendations to observed ServiceNow behavior or configuration
- When proposing a fix or enhancement, connect it to the relevant ServiceNow records, configuration, or implementation details discovered during investigation
