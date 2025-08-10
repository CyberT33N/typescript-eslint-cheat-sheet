### What lives where

- ASTUtils (AST-level helpers)
  - Token and trivia helpers: e.g., `ASTUtils.isOpeningParenToken`, `ASTUtils.isClosingParenToken` (as used in your rule).
  - Pattern/lexeme helpers: `PatternMatcher`, `predicates`.
  - Scope and reference analysis: `ReferenceTracker`, `scopeAnalysis`.
  - General AST helpers: `helpers`, `misc`, `predicates`.
  - When to use: Navigating/manipulating AST, matching patterns, tracking references, token/whitespace decisions.

- ESLintUtils (rule scaffolding + TS integration)
  - Rule creation and typing: `ESLintUtils.RuleCreator` (typed messages/options, docs URL builder).
  - Type services access: `ESLintUtils.getParserServices` (safe, typed access to `program`, `esTreeNodeToTSNodeMap`).
  - Utilities: `applyDefault`, `deepMerge`, `nullThrows`, `parserSeemsToBeTSESLint`, `InferTypesFromRule`.
  - When to use: Creating rules with strict typing, integrating TypeScript type info, safe parser checks, robust rule DX.

- Types you’ll also use
  - `TSESLint` (rule types, `RuleContext`, `RuleFixer`, `SourceCode`, etc.)
  - `TSESTree` (AST node types)
  - `JSONSchema` (for `meta.schema` typing, if desired)

- - - 

### Best-practice: Which to use for what

- MUST use `ESLintUtils.RuleCreator` to define rules (typed `Options`/`MessageIds`, easy docs URL, consistent metadata).
- MUST use `ESLintUtils.getParserServices(context)` for type-aware rules instead of directly reading `context.parserServices`.
- MUST use `ASTUtils` for AST/token-level operations (token classification, reference tracking, pattern matching) and whitespace/format rules.
- SHOULD type rule interfaces with `TSESLint.RuleContext<MessageIds, Options>` and node types with `TSESTree`.
- SHOULD keep visitors narrow and bail out early for performance (selectors, quick guards, compute tokens once).
- SHOULD avoid calling expensive TS APIs in hot loops; cache `typeChecker` from `getParserServices` once.
- SHOULD provide a `RuleCreator` docs URL to link rule docs in diagnostics.
- MUST NOT reimplement parser-service access or AST utilities provided by the utils package.
- MUST NOT access `context.parserServices` without validating TS parser (use `getParserServices` which asserts and throws with a clear message).

- - - 

### Quick decision matrix

- Pure AST/format/whitespace rule (no TS types needed): ESLintUtils.RuleCreator for scaffolding + ASTUtils for AST.
- Type-aware rule (needs TS checker/types): ESLintUtils.RuleCreator + ESLintUtils.getParserServices + ASTUtils as needed.
- Cross-scope or global reference tracking: Use `ASTUtils.ReferenceTracker` and `scopeAnalysis`.

- - - 

### Summary (what the agent MUST do)

- Use `ESLintUtils.RuleCreator` to define every rule.
- Use `ESLintUtils.getParserServices(context)` whenever TypeScript type info is required.
- Use `ASTUtils` for AST/token operations, reference tracking, and pattern utilities.
- Type everything with `TSESLint`/`TSESTree`; keep visitors narrow; avoid hot-loop type checks.
