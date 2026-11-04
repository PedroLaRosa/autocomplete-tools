# Fig Autocomplete-Tooling Repo

This repo contains the source for all of Fig tools related with [autocomplete](https://github.com/withfig/autocomplete).

1. You can see the list of Fig's packages on the NPM registry here: https://www.npmjs.com/~withfig

2. You can see the source code and related README for each package in the `./packages` folder

## Packages

- [`create-completion-spec`](cli/create-completion-spec/README.md)
- [`@tabsmith/publish-spec`](cli/publih-spec-to-team/README.md)
- [`@tabsmith/autocomplete-tools`](cli/tools-cli/README.md)
- [`@tabsmith/eslint-config-autocomplete`](eslint/config-autocomplete/README.md)
- [`@tabsmith/eslint-plugin-fig-linter`](eslint/plugin-fig-linter/README.md)
- [`@tabsmith/autocomplete-generators`](generators/README.md)
- [`@tabsmith/autocomplete-hooks`](hooks/README.md)
- [`@tabsmith/autocomplete-helpers`](helpers/README.md)
- [`argparse_complete_fig`](integrations/argparse/README.md)
- [`cement_complete_fig`](integrations/cement/README.md)
- [`clap_complete_fig`](integrations/clap/README.md)
- [`click_complete_fig`](integrations/click/README.md)
- [`cobracompletefig`](integrations/cobra/README.md)
- [`@tabsmith/complete-commander`](integrations/commander/README.md)
- [`@tabsmith/complete-oclif`](integrations/oclif/README.md)
- [`swift-argument-parser`](integrations/swift-argument-parser/README.md)
- [`@tabsmith/autocomplete-merge`](merge/README.md)
- [`@tabsmith/autocomplete-shared`](shared/README.md)
- [`@tabsmith/autocomplete-types`](types/README.md)

## To publish a package:

npm packages are published by CI. Bump `version` in the package's `package.json` and merge to `main`: the
[Publish packages](.github/workflows/publish.yml) workflow runs `pnpm -r publish`, which publishes every
workspace package whose current version is not on npm yet (it needs the `NPM_TOKEN` repo secret).

Non-npm packages (Go, Python) are still released by git tag, following each package's previous tags
(e.g. `packages/cobra/v1.2.0`, `argparse_complete_fig@1.0.1`).

## Conventions for new CLI integration

### Package (integration) name

- If namespaced it SHOULD be `@tabsmith/complete[-_]($FRAMEWORK_NAME)`
- If not namespaced it SHOULD be `($FRAMEWORK_NAME)[-_]complete[-_]fig`

According to language conventions you can use a dash or an underscore to separate the words.

Examples:
- `@tabsmith/complete-commander`
- `@tabsmith/complete-oclif`
- `clap_complete_fig`
- `cobracompletefig`

### Name of the default subcommand added to the CLI

Most of our CLI integration tools allow to set the name of the subcommand added to the CLI but we also provide a default value for that.
That default name MUST be `generate-fig-spec` such that running `$CLI generate-fig-spec` prints the spec.

### Function that adds the command

The functions exported from the integration can:
- Create a new framework subcommand which will print the spec when invoked
- Generate a spec and return it

In all the cases the names are standardized and SHOULD be:
- `addCompletionSpecCommand` or `createCompletionSpecCommand` for functions creating a new subcommand
- `generateCompletionSpec` for functions that return the spec as a string

According to language conventions these function names can be transformed to snake case, etc...

If the CLI tool integration gets added directly to the module of the CLI tool itself and the CLI tool is configured using chained methods (e.g. yargs), then the chain method name SHOULD be:
- `.figCompletion()`

### Docs on `public-site-nextjs`

Docs MUST conform to the rules listed above too.