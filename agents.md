# Agents

This document describes the automated agents, tools, and workflows used in the openai-langchainjs repository.

## Agents

| Name                        | Description                                                                                                                | Inputs                                                                        | Outputs                                                                            | Permissions                                                           |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| Build Agent                 | Compiles TypeScript source code to JavaScript using the TypeScript compiler. Triggered via `npm run build` or `npm start`. | TypeScript source files in `src/`, `tsconfig.json` configuration              | Compiled JavaScript files in `dist/` directory                                     | Read access to source files, write access to dist directory           |
| Lint Agent                  | Performs static code analysis using XO linter to enforce code style and quality standards.                                 | JavaScript/TypeScript files, XO configuration in `package.json`               | Lint results, exit codes, formatted error messages                                 | Read access to source files                                           |
| Format Agent                | Automatically formats code using Prettier to maintain consistent style across the codebase.                                | Source files (JS, TS, MD, YAML, HTML, CSS), Prettier config in `package.json` | Formatted source files                                                             | Read/write access to source files                                     |
| Pre-commit Agent            | Runs lint-staged to automatically lint and format staged files before commits using simple-git-hooks.                      | Staged git files, lint-staged configuration                                   | Linted and formatted files, commit success/failure                                 | Read/write access to staged files, git hooks                          |
| Azure Deployment Agent      | Provisions and manages Azure OpenAI resources using Azure Developer CLI and Bicep templates.                               | Azure credentials, Bicep templates in `infra/`, `azure.yaml` configuration    | Azure resources (OpenAI service, resource groups), environment variables in `.env` | Azure subscription access, resource creation/modification permissions |
| Credential Management Agent | Handles Azure authentication using DefaultAzureCredential for accessing Azure OpenAI services.                             | Azure credentials (managed identity, service principal, or user credentials)  | Bearer tokens for Azure Cognitive Services                                         | Azure Cognitive Services access, token generation                     |

### Related Files

- **Build**: [`package.json`](./package.json) (scripts), [`tsconfig.json`](./tsconfig.json)
- **Linting**: [`package.json`](./package.json) (XO configuration)
- **Formatting**: [`package.json`](./package.json) (Prettier configuration)
- **Pre-commit**: [`package.json`](./package.json) (simple-git-hooks, lint-staged)
- **Azure Deployment**: [`azure.yaml`](./azure.yaml), [`infra/main.bicep`](./infra/main.bicep), [`infra/main.parameters.json`](./infra/main.parameters.json)
- **Credentials**: [`src/utils/credential-utils.ts`](./src/utils/credential-utils.ts)

### Security Notes

- Azure Deployment Agent requires high-privilege access and should be reviewed for security implications
- Credential Management Agent follows Azure best practices using DefaultAzureCredential
- Pre-commit Agent has write access to source files but operates only on staged changes
