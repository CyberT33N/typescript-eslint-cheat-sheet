### Agent Policy: Custom ESLint Rule Authoring (ASTUtils vs ESLintUtils)

- **Authoritative decision**: Always scaffold rules with `ESLintUtils.RuleCreator`. Use `ASTUtils` for AST/token/scope utilities. Use `ESLintUtils.getParserServices` when TypeScript type info is required.

### When to use ASTUtils

- **Token helpers**: `ASTUtils.isOpeningParenToken`, `ASTUtils.isClosingParenToken` for precise token classification.
- **Pattern/lexeme helpers**: `ast-utils/predicates`, `PatternMatcher` for matching identifiers, literals, and shapes.
- **Reference/scope analysis**: `ReferenceTracker`, `scopeAnalysis` to find global or module-level symbol usage and bindings.
- **AST helpers**: `ast-utils/helpers`, `misc` for safe node/token navigation.
- **Formatting/whitespace rules**: Prefer AST/token utilities for reliable fixes instead of ad-hoc string ops.

### When to use ESLintUtils

- **Rule scaffolding (MUST)**: `ESLintUtils.RuleCreator` for typed messages/options and a canonical docs URL.
- **Type services (MUST for type-aware rules)**: `ESLintUtils.getParserServices(context)` to safely access `program`, `esTreeNodeToTSNodeMap`; do not read `context.parserServices` directly.
- **Utilities**: `nullThrows` (defensive checks), `applyDefault`/`deepMerge` (options), `parserSeemsToBeTSESLint` (guardrails), `InferTypesFromRule` (type inference support).

### Rule authoring standard (MUST/SHOULD)

- **MUST** use `ESLintUtils.RuleCreator` and provide a stable docs URL.
- **MUST** type `MessageIds` and `Options`; use `TSESTree` for nodes and `TSESLint` for rule types.
- **MUST** use `getParserServices` for any TS type-checker access; fetch checker once, avoid hot-loop TS calls.
- **SHOULD** bail early with cheap guards; compute tokens once; keep selectors narrow.
- **SHOULD** define `meta.schema` precisely; keep fixes idempotent and minimal.
- **MUST NOT** access `context.parserServices` directly or re-implement helpers already in utils.
- **MUST** export rules and plugins via named exports only.

### Before vs After (Best Practice migration)

- Before (manual rule module)

```ts
import { ASTUtils } from '@typescript-eslint/utils'
import type { TSESLint, TSESTree } from '@typescript-eslint/utils'

type MessageIds = 'expectedAfter' | 'expectedBefore'
type Options = [{ minParams?: number }?]

const rule: TSESLint.RuleModule<MessageIds, Options> = {
  create(context) {
    const { sourceCode, options } = context
    const [option] = options
    const minParameters = typeof option?.minParams === 'number' && option.minParams > 0 ? option.minParams : 2

    return {
      FunctionDeclaration(node: TSESTree.FunctionDeclaration) {
        const opening = sourceCode.getTokenBefore(node.params[0], ASTUtils.isOpeningParenToken)
        const closing = sourceCode.getTokenAfter(node.params[node.params.length - 1], ASTUtils.isClosingParenToken)
        // enforce newlines via token checks...
      }
    }
  },
  meta: { /* docs, messages, schema, fixable, type */ },
  defaultOptions: [{ minParams: 2 }]
}

export const functionDefinitionParenNewlinePlugin: TSESLint.FlatConfig.Plugin = {
  rules: { 'function-definition-paren-newline': rule }
}
```

- After (Best Practice with RuleCreator)

```ts
import { ASTUtils, ESLintUtils } from '@typescript-eslint/utils'
import type { TSESLint, TSESTree } from '@typescript-eslint/utils'

type MessageIds = 'expectedAfter' | 'expectedBefore'
type Options = [{ minParams?: number }?]

const createRule = ESLintUtils.RuleCreator(
  name => `https://docs.t33n.software/eslint-rules/${name}`
)

const rule = createRule<Options, MessageIds>({
  name: 'function-definition-paren-newline',
  meta: {
    type: 'layout',
    docs: { description: 'Enforce newlines just inside parentheses for function/method definitions only (not calls), when the number of parameters is ≥ minParams' },
    fixable: 'whitespace',
    messages: {
      expectedAfter: "Expected newline after '(' in function definition",
      expectedBefore: "Expected newline before ')' in function definition"
    },
    schema: [{ type: 'object', additionalProperties: false, properties: { minParams: { type: 'integer', minimum: 1 } } }]
  },
  defaultOptions: [{ minParams: 2 }],
  create(context) {
    const { sourceCode, options } = context
    const [option] = options
    const minParameters = typeof option?.minParams === 'number' && option.minParams > 0 ? option.minParams : 2

    return {
      FunctionDeclaration(node: TSESTree.FunctionDeclaration) {
        if (node.params.length < minParameters) return

        const first = node.params[0]
        const last = node.params[node.params.length - 1]
        const opening = sourceCode.getTokenBefore(first, ASTUtils.isOpeningParenToken)
        const closing = sourceCode.getTokenAfter(last, ASTUtils.isClosingParenToken)
        if (!opening || !closing) return

        const tokenAfterOpen = sourceCode.getTokenAfter(opening, { includeComments: true })
        const tokenBeforeClose = sourceCode.getTokenBefore(closing, { includeComments: true })
        if (!tokenAfterOpen || !tokenBeforeClose) return

        if (opening.loc.end.line === tokenAfterOpen.loc.start.line) {
          context.report({
            node,
            loc: opening.loc,
            messageId: 'expectedAfter',
            fix: fixer => fixer.insertTextAfter(opening, '\n')
          })
        }
        if (tokenBeforeClose.loc.end.line === closing.loc.start.line) {
          context.report({
            node,
            loc: closing.loc,
            messageId: 'expectedBefore',
            fix: fixer => fixer.insertTextBefore(closing, '\n')
          })
        }
      }
    }
  }
})

export const functionDefinitionParenNewlinePlugin: TSESLint.FlatConfig.Plugin = {
  rules: { 'function-definition-paren-newline': rule }
}
```

### Optional: adding TS type-awareness

- When a rule must reason about TS types:
  - Use `const services = ESLintUtils.getParserServices(context)` and then `const checker = services.program.getTypeChecker()`.
  - Map nodes via `services.esTreeNodeToTSNodeMap.get(esNode)`.

### Checklist (agent MUST follow)

- Use `ESLintUtils.RuleCreator` with a valid docs URL.
- Use `ASTUtils` for tokens/patterns/reference tracking; do not reimplement.
- Use `ESLintUtils.getParserServices` for any type-aware logic.
- Strongly type `MessageIds`, `Options`, `meta.schema`.
- Keep visitors minimal, bail out early, avoid hot-loop type-checker calls.
- Export rules/plugins via named exports only.
