# typescript-eslint-cheat-sheet




# Docs
- https://typescript-eslint.io/getting-started

## Install
```shell
npm install --save-dev eslint @eslint/js typescript-eslint eslint-plugin-react eslint-plugin-react-hooks eslint-plugin-import eslint-plugin-jsx-a11y eslint-import-resolver-typescript eslint-import-resolver-node
```

package.json:
```javascript
"scripts": {
    "lint": "eslint . --ext .js,.jsx,.ts,.tsx",
    "lint:fix": "eslint . --ext .js,.jsx,.ts,.tsx --fix",
},
```


### eslint.config.mjs:



#### Extrem strict:

<details><summary>Click to expand..</summary>

```typescript
/*
███████████████████████████████████████████████████████████████████████████████
██******************** PRESENTED BY t33n Software ***************************██
██                                                                           ██
██                  ████████╗██████╗ ██████╗ ███╗   ██╗                      ██
██                  ╚══██╔══╝╚════██╗╚════██╗████╗  ██║                      ██
██                     ██║    █████╔╝ █████╔╝██╔██╗ ██║                      ██
██                     ██║    ╚═══██╗ ╚═══██╗██║╚██╗██║                      ██
██                     ██║   ██████╔╝██████╔╝██║ ╚████║                      ██
██                     ╚═╝   ╚═════╝ ╚═════╝ ╚═╝  ╚═══╝                      ██
██                                                                           ██
███████████████████████████████████████████████████████████████████████████████
███████████████████████████████████████████████████████████████████████████████
*/

// ==== Imports ====
import eslint from '@eslint/js'
import importPlugin from 'eslint-plugin-import'
import a11yPlugin from 'eslint-plugin-jsx-a11y'
import reactPlugin from 'eslint-plugin-react'
import reactHooksPlugin from 'eslint-plugin-react-hooks'
import stylistic from '@stylistic/eslint-plugin'
import tseslint from 'typescript-eslint'

export default tseslint.config(
    {
        // Global ignores for other directories, but not for eslint.config.mjs itself regarding naming conventions
        ignores: ['eslint.config.mjs', 'coverage/**']
    },
    // ===== ESLINT BASE RULES =====
    eslint.configs.recommended,
    
    // ===== TYPESCRIPT-ESLINT CONFIGURATIONS =====
    // Include ALL strict TypeScript rules (includes recommended)
    tseslint.configs.strictTypeChecked,
    tseslint.configs.stylisticTypeChecked,
    
    // ===== ESLINT CORE RULES CUSTOMIZATION =====
    {
        rules: {
            // Migrated to @stylistic - now commented out
            // 'arrow-parens': ['error', 'as-needed'],
            'no-var': 'error',
            'no-eval': 'error',
            // 'indent': ['error', 4], // Migrated to @stylistic
            // 'quotes': ['error', 'single'], // Migrated to @stylistic
            'no-console': ['error', { allow: ['warn', 'error', 'info', 'trace' ] }], // Stricter than original
            // 'space-before-function-paren': ['error', 'never'], // Migrated to @stylistic
            // 'padded-blocks': ['error', 'never'], // Migrated to @stylistic
            'prefer-arrow-callback': ['error', { // Stricter than original
                allowNamedFunctions: true
            }],
            'func-names': ['error', 'never'],
            'no-use-before-define': 'off', // Let typescript-eslint handle this
            // 'object-curly-spacing': ['error', 'always'], // Migrated to @stylistic
            // 'comma-dangle': ['error', 'never'], // Migrated to @stylistic
            // 'semi': ['error', 'never'], // Migrated to @stylistic
            'new-cap': ['error', { // Stricter than original
                newIsCap: true,
                capIsNew: false
            }],
            'one-var': ['error', 'never'], // Stricter than original
            'guard-for-in': 'error', // Stricter than original
            'no-duplicate-imports': 'error',
            'no-return-await': 'error',
            'no-template-curly-in-string': 'error',
            'require-atomic-updates': 'error',
            'accessor-pairs': 'error',
            'array-callback-return': 'error',
            'block-scoped-var': 'error',
            'camelcase': ['error', { properties: 'never' }],
            'complexity': ['error', 20],
            'consistent-return': 'error',
            'curly': ['error', 'all'],
            'default-case': 'error',
            'eqeqeq': ['error', 'always'],
            'dot-notation': 'off' // Disabled to allow bracket notation for private method testing
        }
    },
    
    // ===== IMPORT PLUGIN =====
    {
        plugins: {
            import: importPlugin
        },
        settings: {
            'import/parsers': {
                '@typescript-eslint/parser': ['.ts', '.tsx']
            },
            'import/resolver': {
                typescript: {
                    alwaysTryTypes: true,
                    project: './tsconfig.json',
                    extensions: ['.ts', '.tsx', '.js', '.jsx'],
                    paths: {
                        '@main/*': ['./src/main/*'],
                        '@/*': ['./src/*']
                    }
                },
                node: {
                    extensions: ['.ts', '.tsx', '.js', '.jsx'],
                    paths: ['src']
                }
            }
        },
        rules: {
            'import/first': 'error',
            'import/no-duplicates': 'error',
            'import/order': ['error', {
                'groups': [
                    'builtin',
                    'external',
                    'internal',
                    'parent',
                    'sibling',
                    'index'
                ],
                'alphabetize': {
                    'order': 'asc',
                    'caseInsensitive': true
                }
            }],
            'import/no-unresolved': 'error',
            'import/no-cycle': 'error',
            'import/no-unused-modules': 'error'
        }
    },
    
    // ===== ENTERPRISE-GRADE @STYLISTIC CONFIGURATION =====
    {
        plugins: {
            '@stylistic': stylistic
        },
        rules: {
            // ===== SPACING & INDENTATION =====
            '@stylistic/indent': ['error', 4, { 
                'SwitchCase': 1,
                'VariableDeclarator': 1,
                'outerIIFEBody': 1,
                'MemberExpression': 1,
                'FunctionDeclaration': {
                    'parameters': 1,
                    'body': 1
                },
                'FunctionExpression': {
                    'parameters': 1,
                    'body': 1
                },
                'CallExpression': {
                    'arguments': 1
                },
                'ArrayExpression': 1,
                'ObjectExpression': 1,
                'ImportDeclaration': 1,
                'flatTernaryExpressions': false,
                'offsetTernaryExpressions': true,
                'ignoreComments': false
            }],
            '@stylistic/indent-binary-ops': ['error', 4],
            '@stylistic/key-spacing': ['error', {
                'beforeColon': false,
                'afterColon': true,
                'mode': 'strict'
            }],
            '@stylistic/keyword-spacing': ['error', {
                'before': true,
                'after': true,
                'overrides': {
                    'return': { 'after': true },
                    'throw': { 'after': true },
                    'case': { 'after': true }
                }
            }],
            '@stylistic/space-before-blocks': ['error', 'always'],
            '@stylistic/space-before-function-paren': ['error', {
                'anonymous': 'never',
                'named': 'never',
                'asyncArrow': 'always'
            }],
            '@stylistic/space-in-parens': ['error', 'never'],
            '@stylistic/space-infix-ops': ['error', { 'int32Hint': false }],
            '@stylistic/space-unary-ops': ['error', {
                'words': true,
                'nonwords': false,
                'overrides': {}
            }],
            '@stylistic/spaced-comment': ['error', 'always', {
                'line': {
                    'exceptions': ['-', '+'],
                    'markers': ['=', '!', '/']
                },
                'block': {
                    'exceptions': ['-', '+'],
                    'markers': ['=', '!', ':', '::'],
                    'balanced': true
                }
            }],
            
            // ===== LINE BREAKS & WRAPPING =====
            '@stylistic/max-len': ['error', {
                'code': 120,
                'tabWidth': 4,
                'ignoreUrls': true,
                'ignoreStrings': false,
                'ignoreTemplateLiterals': false,
                'ignoreRegExpLiterals': true,
                'ignoreComments': true
            }],
            '@stylistic/max-statements-per-line': ['error', { 'max': 1 }],
            '@stylistic/newline-per-chained-call': ['error', { 'ignoreChainWithDepth': 2 }],
            '@stylistic/operator-linebreak': ['error', 'before', {
                'overrides': {
                    '=': 'none',
                    '+=': 'none',
                    '-=': 'none',
                    '*=': 'none',
                    '/=': 'none',
                    '%=': 'none'
                }
            }],
            '@stylistic/linebreak-style': ['error', 'unix'],
            '@stylistic/eol-last': ['error', 'always'],
            '@stylistic/no-multiple-empty-lines': ['error', {
                'max': 1,
                'maxEOF': 0,
                'maxBOF': 0
            }],
            '@stylistic/no-trailing-spaces': ['error', {
                'skipBlankLines': false,
                'ignoreComments': false
            }],
            '@stylistic/nonblock-statement-body-position': ['error', 'below'],
            
            // ===== ARRAYS =====
            '@stylistic/array-bracket-newline': ['error', {
                'multiline': true,
                'minItems': 3
            }],
            '@stylistic/array-bracket-spacing': ['error', 'never'],
            '@stylistic/array-element-newline': ['error', {
                'multiline': true,
                'minItems': 3
            }],
            
            // ===== OBJECTS =====
            '@stylistic/object-curly-spacing': ['error', 'always'],
            '@stylistic/object-curly-newline': ['error', {
                'ObjectExpression': { 
                    'multiline': true, 
                    'minProperties': 2,
                    'consistent': true
                },
                'ObjectPattern': { 
                    'multiline': true,
                    'minProperties': 2,
                    'consistent': true
                },
                'ImportDeclaration': { 
                    'multiline': true,
                    'minProperties': 3,
                    'consistent': true
                },
                'ExportDeclaration': { 
                    'multiline': true,
                    'minProperties': 3,
                    'consistent': true
                }
            }],
            '@stylistic/object-property-newline': ['error', { 
                'allowAllPropertiesOnSameLine': false
            }],
            '@stylistic/quote-props': ['error', 'as-needed', {
                'keywords': false,
                'unnecessary': true,
                'numbers': false
            }],
            
            // ===== FUNCTIONS =====
            '@stylistic/function-paren-newline': ['error', { 
                'minItems': 1
            }],
            '@stylistic/function-call-argument-newline': ['error', 'consistent'],
            '@stylistic/function-call-spacing': ['error', 'never'],
            '@stylistic/arrow-parens': ['error', 'as-needed', {
                'requireForBlockBody': true
            }],
            '@stylistic/arrow-spacing': ['error', {
                'before': true,
                'after': true
            }],
            '@stylistic/implicit-arrow-linebreak': ['error', 'beside'],
            '@stylistic/wrap-iife': ['error', 'inside', {
                'functionPrototypeMethods': true
            }],
            
            // ===== BLOCKS & BRACES =====
            '@stylistic/brace-style': ['error', 'stroustrup', { 
                'allowSingleLine': false
            }],
            '@stylistic/block-spacing': ['error', 'always'],
            '@stylistic/padded-blocks': ['error', 'never', {
                'allowSingleLineBlocks': false
            }],
            '@stylistic/curly-newline': ['error'],
            
            // ===== PUNCTUATION =====
            '@stylistic/comma-dangle': ['error', {
                'arrays': 'never',
                'objects': 'never',
                'imports': 'never',
                'exports': 'never',
                'functions': 'never'
            }],
            '@stylistic/comma-spacing': ['error', {
                'before': false,
                'after': true
            }],
            '@stylistic/comma-style': ['error', 'last'],
            '@stylistic/semi': ['error', 'never', {
                'beforeStatementContinuationChars': 'never'
            }],
            '@stylistic/semi-spacing': ['error', {
                'before': false,
                'after': true
            }],
            '@stylistic/semi-style': ['error', 'last'],
            '@stylistic/quotes': ['error', 'single', {
                'avoidEscape': true,
                'allowTemplateLiterals': 'never'
            }],
            '@stylistic/template-curly-spacing': ['error', 'never'],
            '@stylistic/template-tag-spacing': ['error', 'never'],
            
            // ===== MISC FORMATTING =====
            '@stylistic/computed-property-spacing': ['error', 'never'],
            '@stylistic/dot-location': ['error', 'property'],
            '@stylistic/generator-star-spacing': ['error', {
                'before': true,
                'after': false
            }],
            '@stylistic/yield-star-spacing': ['error', {
                'before': false,
                'after': true
            }],
            '@stylistic/switch-colon-spacing': ['error', {
                'before': false,
                'after': true
            }],
            '@stylistic/no-confusing-arrow': ['error', {
                'allowParens': true
            }],
            '@stylistic/no-extra-parens': ['error', 'all', {
                'conditionalAssign': false,
                'returnAssign': false,
                'nestedBinaryExpressions': false,
                'ignoreJSX': 'all',
                'enforceForArrowConditionals': false,
                'enforceForSequenceExpressions': false,
                'enforceForNewInMemberExpressions': false,
                'enforceForFunctionPrototypeMethods': false
            }],
            '@stylistic/no-extra-semi': ['error'],
            '@stylistic/no-floating-decimal': ['error'],
            '@stylistic/no-mixed-operators': ['error', {
                'groups': [
                    ['%', '**'],
                    ['%', '+'],
                    ['%', '-'],
                    ['%', '*'],
                    ['%', '/'],
                    ['/', '*'],
                    ['&', '|', '<<', '>>', '>>>'],
                    ['==', '!=', '===', '!=='],
                    ['&&', '||']
                ],
                'allowSamePrecedence': true
            }],
            '@stylistic/no-mixed-spaces-and-tabs': ['error'],
            '@stylistic/no-multi-spaces': ['error', {
                'ignoreEOLComments': false,
                'exceptions': {}
            }],
            '@stylistic/no-tabs': ['error'],
            '@stylistic/no-whitespace-before-property': ['error'],
            '@stylistic/rest-spread-spacing': ['error', 'never'],
            '@stylistic/wrap-regex': ['error'],
            
            // ===== TYPESCRIPT SPECIFIC =====
            '@stylistic/type-annotation-spacing': ['error', {
                'before': false,
                'after': true,
                'overrides': {
                    'arrow': {
                        'before': true,
                        'after': true
                    }
                }
            }],
            '@stylistic/type-generic-spacing': ['error'],
            '@stylistic/type-named-tuple-spacing': ['error'],
            '@stylistic/member-delimiter-style': ['error', {
                'multiline': {
                    'delimiter': 'none',
                    'requireLast': false
                },
                'singleline': {
                    'delimiter': 'semi',
                    'requireLast': false
                }
            }],
            
            // ===== JSX/REACT SPECIFIC (only essential for future React support) =====
            '@stylistic/jsx-quotes': ['error', 'prefer-double'],
            '@stylistic/jsx-closing-bracket-location': ['error', 'line-aligned'],
            '@stylistic/jsx-closing-tag-location': ['error'],
            '@stylistic/jsx-curly-spacing': ['error', {
                'when': 'never',
                'children': true
            }],
            '@stylistic/jsx-curly-newline': ['error', {
                'multiline': 'consistent',
                'singleline': 'forbid'
            }],
            '@stylistic/jsx-equals-spacing': ['error', 'never'],
            '@stylistic/jsx-first-prop-new-line': ['error', 'multiline'],
            // '@stylistic/jsx-indent': deprecated - use '@stylistic/indent' instead
            '@stylistic/jsx-indent-props': ['error', 4],
            '@stylistic/jsx-max-props-per-line': ['error', {
                'maximum': 1,
                'when': 'multiline'
            }],
            '@stylistic/jsx-one-expression-per-line': ['error', {
                'allow': 'single-child'
            }],
            '@stylistic/jsx-props-no-multi-spaces': ['error'],
            '@stylistic/jsx-tag-spacing': ['error', {
                'closingSlash': 'never',
                'beforeSelfClosing': 'always',
                'afterOpening': 'never',
                'beforeClosing': 'never'
            }],
            '@stylistic/jsx-wrap-multilines': ['error', {
                'declaration': 'parens-new-line',
                'assignment': 'parens-new-line',
                'return': 'parens-new-line',
                'arrow': 'parens-new-line',
                'condition': 'parens-new-line',
                'logical': 'parens-new-line',
                'prop': 'parens-new-line'
            }],
            '@stylistic/jsx-self-closing-comp': ['error', {
                'component': true,
                'html': true
            }],
            '@stylistic/jsx-sort-props': ['error', {
                'callbacksLast': true,
                'shorthandFirst': true,
                'multiline': 'last',
                'ignoreCase': true,
                'reservedFirst': true
            }],
            '@stylistic/jsx-pascal-case': ['error', {
                'allowAllCaps': false,
                'allowNamespace': true
            }],
            '@stylistic/jsx-child-element-spacing': ['error'],
            '@stylistic/jsx-curly-brace-presence': ['error', {
                'props': 'never',
                'children': 'never'
            }],
            '@stylistic/jsx-function-call-newline': ['error', 'multiline'],
            // Note: jsx-newline rule removed due to compatibility issues
            
            // ===== COMMENTS & DOCUMENTATION =====
            '@stylistic/line-comment-position': ['error', {
                'position': 'above',
                'ignorePattern': 'eslint|jshint|global',
                'applyDefaultIgnorePatterns': true
            }],
            '@stylistic/lines-around-comment': ['error', {
                'beforeBlockComment': true,
                'afterBlockComment': false,
                'beforeLineComment': true,
                'afterLineComment': false,
                'allowBlockStart': true,
                'allowBlockEnd': true,
                'allowObjectStart': true,
                'allowObjectEnd': true,
                'allowArrayStart': true,
                'allowArrayEnd': true,
                'allowClassStart': true,
                'allowClassEnd': true,
                'applyDefaultIgnorePatterns': true
            }],
            '@stylistic/multiline-comment-style': ['error', 'starred-block'],
            
            // ===== CLASS MEMBERS =====
            '@stylistic/lines-between-class-members': ['error', 'always', {
                'exceptAfterSingleLine': false,
                'exceptAfterOverload': true
            }],
            '@stylistic/padding-line-between-statements': ['error',
                { 'blankLine': 'always', 'prev': 'directive', 'next': '*' },
                { 'blankLine': 'any', 'prev': 'directive', 'next': 'directive' },
                { 'blankLine': 'always', 'prev': ['const', 'let', 'var'], 'next': '*' },
                { 'blankLine': 'any', 'prev': ['const', 'let', 'var'], 'next': ['const', 'let', 'var'] },
                { 'blankLine': 'always', 'prev': '*', 'next': 'return' },
                { 'blankLine': 'always', 'prev': '*', 'next': ['if', 'try', 'class', 'export'] },
                { 'blankLine': 'always', 'prev': ['if', 'try', 'class', 'export'], 'next': '*' },
                { 'blankLine': 'any', 'prev': ['export'], 'next': ['export'] }
            ],
            
            // ===== TERNARY =====
            '@stylistic/multiline-ternary': ['error', 'always-multiline'],
            '@stylistic/new-parens': ['error', 'always'],
            '@stylistic/one-var-declaration-per-line': ['error', 'always']
        }
    },
    
    // ===== REACT RULES =====
    {
        plugins: {
            react: reactPlugin,
            'react-hooks': reactHooksPlugin,
            'jsx-a11y': a11yPlugin
        },
        languageOptions: {
            parserOptions: {
                ecmaFeatures: {
                    jsx: true
                }
            }
        },
        settings: {
            react: {
                version: 'detect'
            }
        },
        rules: {
            // React rules
            'react/react-in-jsx-scope': 'off',
            'react/prop-types': 'off',
            'react-hooks/rules-of-hooks': 'error',
            'react-hooks/exhaustive-deps': 'error', // Stricter than original
            'react/no-access-state-in-setstate': 'error',
            'react/no-array-index-key': 'error',
            'react/no-danger': 'error',
            'react/no-did-mount-set-state': 'error',
            'react/no-did-update-set-state': 'error',
            'react/no-direct-mutation-state': 'error',
            'react/no-redundant-should-component-update': 'error',
            'react/no-typos': 'error',
            'react/no-this-in-sfc': 'error',
            'react/no-unescaped-entities': 'error',
            'react/no-unknown-property': 'error',
            'react/no-unused-state': 'error',
            'react/no-will-update-set-state': 'error',
            'react/prefer-es6-class': ['error', 'always'],
            'react/prefer-stateless-function': 'error',
            'react/self-closing-comp': 'error',
            'react/sort-comp': 'error',
            'react/jsx-no-bind': ['error', {
                'allowArrowFunctions': true
            }],
            'react/jsx-no-useless-fragment': 'error',
            'react/jsx-pascal-case': 'error',
            
            // A11y rules
            'jsx-a11y/alt-text': 'error',
            'jsx-a11y/anchor-has-content': 'error',
            'jsx-a11y/anchor-is-valid': 'error',
            'jsx-a11y/aria-props': 'error',
            'jsx-a11y/aria-role': 'error',
            'jsx-a11y/heading-has-content': 'error',
            'jsx-a11y/no-autofocus': 'error',
            'jsx-a11y/no-redundant-roles': 'error'
        }
    },
    
    // ===== TYPESCRIPT PARSER CONFIG =====
    {
        languageOptions: {
            parser: tseslint.parser,
            parserOptions: {
                /*
                - https://typescript-eslint.io/blog/announcing-typescript-eslint-v8/#project-service
                The project service will automatically find the closest tsconfig.json for each file (like project: true)
                */
                projectService: true,

                /* 
                - https://typescript-eslint.io/packages/parser/#tsconfigrootdir
                The root directory for the tsconfig.json file (https://typescript-eslint.io/packages/parser/#tsconfigrootdir) */
                tsconfigRootDir: import.meta.dirname
            }
        }
    },
    
    // ===== ADDITIONAL TYPESCRIPT RULES =====
    {
        rules: {
            // Additional typescript-eslint rules not included in strict
            '@typescript-eslint/explicit-function-return-type': 'error',
            '@typescript-eslint/explicit-member-accessibility': 'error',
            '@typescript-eslint/member-ordering': 'error',
            '@typescript-eslint/dot-notation': 'off', // Disabled to allow bracket notation for private method testing
            '@typescript-eslint/naming-convention': [
                'error',
                {
                    'selector': 'default',
                    'format': ['camelCase']
                },
                {
                    'selector': 'variable',
                    'format': ['camelCase', 'UPPER_CASE']
                },
                {
                    'selector': 'parameter',
                    'format': ['camelCase'],
                    'leadingUnderscore': 'allow'
                },
                {
                    'selector': 'memberLike',
                    'modifiers': ['private'],
                    'format': ['camelCase'],
                    'leadingUnderscore': 'require'
                },
                {
                    'selector': 'typeLike',
                    'format': ['PascalCase']
                },
                {
                    'selector': 'interface',
                    'format': ['PascalCase'],
                    'prefix': ['I']
                },
                {
                    'selector': 'enum',
                    'format': ['PascalCase'],
                    'prefix': ['E']
                },
                // ANPASSUNG FÜR OBJEKT-PROPERTIES (wie _errors oder Zods required_error)
                {
                    'selector': ['objectLiteralProperty', 'typeProperty'], // Gilt für Properties in Objektliteralen und Typdefinitionen
                    'format': ['camelCase', 'snake_case', 'PascalCase'], // Erlaube verschiedene Formate
                    'leadingUnderscore': 'allow' // WICHTIG: Erlaube hier führende Unterstriche
                // Optional: Wenn du es *nur* für spezifische Namen wie '_errors' erlauben willst:
                // 'filter': { 'regex': '^_errors$', 'match': true }
                // Aber 'allow' ist oft einfacher, wenn mehrere solcher Fälle von externen Bibliotheken kommen.
                }
            ],
            '@typescript-eslint/no-explicit-any': 'error',
            '@typescript-eslint/no-non-null-assertion': 'error',
            '@typescript-eslint/no-unnecessary-condition': 'error',
            '@typescript-eslint/prefer-optional-chain': 'error',
            '@typescript-eslint/prefer-nullish-coalescing': 'error',
            '@typescript-eslint/prefer-readonly': 'error',
            '@typescript-eslint/prefer-readonly-parameter-types': 'error',
            '@typescript-eslint/require-array-sort-compare': 'error',
            '@typescript-eslint/strict-boolean-expressions': 'error',
            '@typescript-eslint/switch-exhaustiveness-check': 'error',
            '@typescript-eslint/restrict-template-expressions': 'error',
            '@typescript-eslint/unbound-method': 'error',
            '@typescript-eslint/no-floating-promises': 'error',
            '@typescript-eslint/promise-function-async': 'error',
            '@typescript-eslint/prefer-enum-initializers': 'error',
            '@typescript-eslint/prefer-literal-enum-member': 'error',
            
            // ===== ENTERPRISE-GRADE ZUSÄTZLICHE REGELN =====
            // Type Safety Enhancement
            '@typescript-eslint/no-unsafe-type-assertion': 'error', // Verhindert unsichere Type Assertions
            '@typescript-eslint/no-unnecessary-type-conversion': 'error', // Verhindert unnötige Type Conversions
            '@typescript-eslint/consistent-return': 'error', // Erzwingt konsistente Return Types
            '@typescript-eslint/no-unnecessary-parameter-property-assignment': 'error', // Verhindert redundante Zuweisungen
            
            // Import/Export Hygiene (Google/Microsoft Standards)
            '@typescript-eslint/no-import-type-side-effects': 'error', // Performance: Verhindert Side Effects bei Type Imports
            '@typescript-eslint/consistent-type-exports': 'error', // Konsistente Type Exports
            '@typescript-eslint/no-useless-empty-export': 'error', // Verhindert leere Exports
            
            // Code Quality & Maintainability
            '@typescript-eslint/no-unnecessary-qualifier': 'error', // Entfernt unnötige Namespace Qualifier
            '@typescript-eslint/prefer-destructuring': ['error', { // Erzwingt Destructuring (moderne Syntax)
                'array': true,
                'object': true
            }],
            '@typescript-eslint/parameter-properties': ['error', { // Explizite Parameter Properties
                'prefer': 'parameter-property'
            }],
            
            // Restricted Types (Security & Type Safety)
            '@typescript-eslint/no-restricted-types': ['error', {
                'types': {
                    'Object': {
                        'message': 'Use Record<string, unknown> or a specific interface instead',
                        'fixWith': 'Record<string, unknown>'
                    },
                    'Function': {
                        'message': 'Use a specific function type instead',
                        'suggest': ['() => void', '(...args: unknown[]) => unknown']
                    },
                    '{}': {
                        'message': 'Use Record<string, never> for empty object, unknown for any value, or a specific interface',
                        'fixWith': 'Record<string, never>'
                    }
                }
            }],
            
            // Method Signature Enforcement
            '@typescript-eslint/method-signature-style': ['error', 'property'], // Konsistente Method Signatures
            
            // Class Design (Enterprise OOP Standards)
            '@typescript-eslint/class-methods-use-this': ['error', { // Statische Methoden wenn kein "this"
                'exceptMethods': ['render', 'componentDidMount', 'componentDidUpdate', 'componentWillUnmount'],
                'enforceForClassFields': true
            }],
            
            // Async/Promise Best Practices
            '@typescript-eslint/return-await': ['error', 'always'], // Explizites await für besseres Stack Tracing
            '@typescript-eslint/no-misused-promises': ['error', {
                'checksVoidReturn': {
                    'arguments': true,
                    'attributes': true,
                    'properties': true,
                    'returns': true,
                    'variables': true
                },
                'checksConditionals': true,
                'checksSpreads': true
            }],
            
            // Enhanced Type Checking für Edge Cases
            '@typescript-eslint/no-confusing-void-expression': ['error', {
                'ignoreArrowShorthand': false,
                'ignoreVoidOperator': false
            }],
            
            // Type Annotation Requirements (für kritische Bereiche)
            '@typescript-eslint/typedef': ['error', {
                'arrayDestructuring': false,
                'arrowParameter': false,
                'memberVariableDeclaration': true, // Klassen-Member müssen typisiert sein
                'objectDestructuring': false,
                'parameter': true, // Funktionsparameter müssen typisiert sein
                'propertyDeclaration': true, // Properties müssen typisiert sein
                'variableDeclaration': false, // Kann durch Type Inference abgeleitet werden
                'variableDeclarationIgnoreFunction': true
            }]
        }
    }
)
```

</details>














#### Recommended:


<details><summary>Click to expand..</summary>

# v2:

<details><summary>Click to expand..</summary>

```typescript
import eslint from '@eslint/js'
import tseslint from 'typescript-eslint'
import reactPlugin from 'eslint-plugin-react'
import reactHooksPlugin from 'eslint-plugin-react-hooks'
import importPlugin from 'eslint-plugin-import' // NEU: Import Plugin hinzugefügt

export default tseslint.config(
    // ===== ESLINT RULES =====
    {
        ...eslint.configs.recommended,
        rules: {
            ...eslint.configs.recommended.rules,

            // Hier fügst du deine neue Regel hinzu
            'arrow-parens': ['error', 'as-needed'],
            'no-var': 1,
            'no-eval': 'error',
            indent: ['error', 4],
            quotes: ['error', 'single'],
            'no-console': 'off',
            'space-before-function-paren': ['error', 'never'],
            'padded-blocks': ['error', 'never'],

            'prefer-arrow-callback': [0, {
                allowNamedFunctions: true
            }],

            'func-names': ['error', 'never'],

            'no-use-before-define': ['error', {
                functions: true,
                classes: true
            }],

            'max-len': ['error', 120],
            'object-curly-spacing': 0,
            'comma-dangle': ['error', 'never'],
            semi: [2, 'never'],
            'new-cap': 0,
            'one-var': 0,
            'guard-for-in': 0,
        }
    },

    // ===== IMPORT PLUGIN ===== (NEU: Import Plugin Konfiguration)
    {
        plugins: {
            import: importPlugin
        },
        settings: {
            'import/parsers': {
                '@typescript-eslint/parser': ['.ts', '.tsx']
            },
            'import/resolver': {
                typescript: {
                    alwaysTryTypes: true,
                    project: './tsconfig.json'
                }
            }
        },
        rules: {
            'import/first': 'error',
            'import/no-duplicates': 'error',
            'import/order': ['error', {
                'groups': [
                    'builtin',
                    'external',
                    'internal',
                    'parent',
                    'sibling',
                    'index'
                ],
                'alphabetize': {
                    'order': 'asc',
                    'caseInsensitive': true
                }
            }],
            'import/no-unresolved': 'error',
            'import/no-cycle': 'error'
        }
    },

    // ===== REACT RULES =====
    {
        plugins: {
            react: reactPlugin,
            'react-hooks': reactHooksPlugin
        },
        languageOptions: {
            parserOptions: {
                ecmaFeatures: {
                    jsx: true
                }
            }
        },
        settings: {
            react: {
                version: 'detect'
            }
        },
        rules: {
            // React rules
            'react/react-in-jsx-scope': 'off',
            'react/prop-types': 'off',
            'react-hooks/rules-of-hooks': 'error',
            'react-hooks/exhaustive-deps': 'warn'
        }
    },

    // ===== TSESLINT RULES =====
    // Use the type-checked configurations
    ...tseslint.configs.recommendedTypeChecked,
    ...tseslint.configs.stylisticTypeChecked,
    
    // Add TypeScript parser configuration
    // ===== TYPESCRIPT PARSER CONFIG =====
    {
        languageOptions: {
            parser: tseslint.parser,
            parserOptions: {
                /*
                - https://typescript-eslint.io/blog/announcing-typescript-eslint-v8/#project-service
                The project service will automatically find the closest tsconfig.json for each file (like project: true)
                */
                projectService: true,

                /* 
                - https://typescript-eslint.io/packages/parser/#tsconfigrootdir
                The root directory for the tsconfig.json file (https://typescript-eslint.io/packages/parser/#tsconfigrootdir) */
                tsconfigRootDir: import.meta.dirname,
            }
        }
    },

    // Custom TypeScript rule overrides
    {
        rules: {
            // Disable for private constructor where we use it for singleton pattern
            '@typescript-eslint/no-empty-function': 'off',

            // Disable for accessing private properties e.g. ModelManager['instance']
            '@typescript-eslint/dot-notation': 'off',

            // Allowing records and index types
            '@typescript-eslint/consistent-indexed-object-style': 'off'
        }
    }
)
```
  
</details>


<br><br>

# old v1:

<details><summary>Click to expand..</summary>

```typescript

import eslint from '@eslint/js'
import tseslint from 'typescript-eslint'

export default tseslint.config(
    // ===== ESLINT RULES =====
    {
        ...eslint.configs.recommended,
        rules: {
            ...eslint.configs.recommended.rules,

            // Hier fügst du deine neue Regel hinzu
            'arrow-parens': ['error', 'as-needed'],
            'no-var': 1,
            'no-eval': 'error',
            indent: ['error', 4],
            quotes: ['error', 'single'],
            'no-console': 'off',
            'space-before-function-paren': ['error', 'never'],
            'padded-blocks': ['error', 'never'],

            'prefer-arrow-callback': [0, {
                allowNamedFunctions: true
            }],

            'func-names': ['error', 'never'],

            'no-use-before-define': ['error', {
                functions: true,
                classes: true
            }],

            'max-len': ['error', 120],
            'object-curly-spacing': 0,
            'comma-dangle': ['error', 'never'],
            semi: [2, 'never'],
            'new-cap': 0,
            'one-var': 0,
            'guard-for-in': 0,
        }
    },

    // ===== TSESLINT RULES =====
    ...tseslint.configs.stylisticTypeChecked,
    ...tseslint.configs.recommendedTypeChecked,
    {
        languageOptions: {
            parserOptions: {
                projectService: true,
                tsconfigRootDir: import.meta.dirname,
            }
        }
    },
    // ...tseslint.configs.recommended,
    // ...tseslint.configs.strictTypeChecked,
    //...tseslint.configs.stylisticTypeChecked

    {
        rules: {
              // Disable for private constructor where we use it for singleton pattern
            '@typescript-eslint/no-empty-function': 'off',

            // Disable for accessing private properties e.g. ModelManager['instance']
            '@typescript-eslint/dot-notation': 'off',

            // Allowing records and index types
            '@typescript-eslint/consistent-indexed-object-style': 'off'
        }
    }
)
```

</details>


</details>
















<br><br>
<br><br>









## Configs
- https://typescript-eslint.io/users/configs/
Recommended Configurations

We recommend that most projects should extend from one of:
    recommended: Recommended rules for code correctness that you can drop in without additional configuration.
    - `export default tseslint.config(...tseslint.configs.recommended);`
    recommended-type-checked: Contains recommended + additional recommended rules that require type information.
    - `export default tseslint.config(...tseslint.configs.recommendedTypeChecked);`
    strict: Contains recommended + additional strict rules that can also catch bugs but are more opinionated than recommended rules.
    - `export default tseslint.config(...tseslint.configs.strict);`
    strict-type-checked: Contains strict + additional strict rules require type information.
    - `export default tseslint.config(...tseslint.configs.strictTypeChecked);`


Additionally, we provide a stylistic config that enforces concise and consistent code. We recommend that most projects should extend from either:
    stylistic: Stylistic rules you can drop in without additional configuration.
     - `export default tseslint.config(...tseslint.configs.stylistic);`
    stylistic-type-checked: Contains stylistic + additional stylistic rules that require type information.
    - `export default tseslint.config(...tseslint.configs.stylisticTypeChecked);`


Notice that type-checked rules need additional settings:
- https://typescript-eslint.io/getting-started/typed-linting/
```typescript
import eslint from '@eslint/js';
import tseslint from 'typescript-eslint';

export default tseslint.config(
  eslint.configs.recommended,

  ...tseslint.configs.recommendedTypeChecked,
  {
    languageOptions: {
      parserOptions: {
        projectService: true,
        tsconfigRootDir: import.meta.dirname,
      },
    },
  },
);
```
- import.meta.dirname is only present for ESM files in Node.js >=20.11.0 / >= 21.2.0.
For CommonJS modules and/or older versions of Node.js, use __dirname or an alternative.
- **If your eslint.config.mjs getting a parsing error then add to your tsconfig.json at the include array `"**/*.mjs",`**








<br><br>
<br><br>

## Rules
- https://typescript-eslint.io/rules/

<br><br>

### no-explicit-any

Comment:
```javascript
/* eslint-disable  @typescript-eslint/no-explicit-any */
```




<br><br>

### unbound-method
- https://typescript-eslint.io/rules/unbound-method/
  
Comment:
```javascript
/* eslint-disable  @typescript-eslint/no-explicit-any */
```

#### Usage with statics
- There are cases e.g. when you create a singleton class with the getInstance method concept when eslint will trigger an error for unbound-method… There you can disable it with this rules. However, the prefered way is to create a static arrow function instead of diable it via rule!
```javascript
// static getInstance() method using this
'@typescript-eslint/unbound-method': [
    'error',
    {
        'ignoreStatic': true
    }
]
```

Correct:
```typescript
  public static getInstance = async(): Promise<ModelManager> => {
        if (!this.instance) {
            this.instance = new ModelManager()
            await this.instance.init()
        }
        
        return this.instance
    }

```

Wrong
```typescript
    public static async getInstance(): Promise<ModelManager> {
        if (!this.instance) {
            this.instance = new ModelManager()
            await this.instance.init()
        }
        
        return this.instance
    }
```

