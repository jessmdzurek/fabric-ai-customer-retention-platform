# 001 - Fabric Git Integration Strategy

## Status

Accepted

## Context

This project uses Microsoft Fabric as the primary analytics engineering platform. Fabric supports Git integration for syncing workspace items such as notebooks, pipelines, lakehouses, warehouses, semantic models, and reports.

The repository also contains hand-authored project documentation, source code, scripts, and future CI/CD assets. To keep the repository organized, Fabric-managed workspace artifacts should be separated from developer-authored source files.

## Decision

The repository will include a dedicated `/fabric-items` folder reserved for Microsoft Fabric Git-integrated workspace artifacts.

For the initial version of the project:

- The Fabric development workspace will connect to the `dev` branch.
- The Fabric workspace Git integration path will be `/fabric-items`.
- Feature development will follow the standard branch pattern: `feature/*` → `dev` → `main`.
- The Fabric workspace will not be connected directly to each feature branch during the initial implementation.

## Rationale

This approach keeps the project simple while still demonstrating common engineering practices.

Connecting the Fabric development workspace to the `dev` branch provides a stable shared integration point for Fabric artifacts. Feature branches can still be used for documentation, scripts, and source-code changes without constantly switching the Fabric workspace between branches.

This avoids unnecessary complexity during the first version of the project while leaving room for more advanced CI/CD patterns later, such as separate Fabric workspaces for development, test, and production.

## Benefits

- Benefits
- Keeps Fabric artifacts isolated from source code and documentation.
- Supports a clean portfolio review experience.
- Avoids over-engineering the first version of the project.
- Leaves room for future CI/CD automation.
- Demonstrates intentional source-control strategy.

## Tradeoffs

- Feature branch isolation for Fabric workspace items is not implemented in version 1.
- Developers must treat the dev branch as the active Fabric integration branch.
- More advanced workspace-per-feature-branch development can be added later if needed.

## Future Considerations

- Separate Fabric workspaces for dev, test, and production.
- Fabric deployment pipelines.
- GitHub Actions/ Azure DevOps for automated validation or deployment.
- Environment-specific parameterization.

## Repo Organization

```text
repo/
├── docs/
├── src/
├── fabric-items/
└── README.md