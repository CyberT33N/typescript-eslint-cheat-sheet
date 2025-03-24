# typescript-eslint-cheat-sheet




# Docs
- https://typescript-eslint.io/getting-started

## Install
```shell
npm install --save-dev eslint @eslint/js typescript-eslint eslint-plugin-react eslint-plugin-react-hooks eslint-plugin-import eslint-plugin-jsx-a11y
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
import eslint from '@eslint/js'
import tseslint from 'typescript-eslint'
import reactPlugin from 'eslint-plugin-react'
import reactHooksPlugin from 'eslint-plugin-react-hooks'
import importPlugin from 'eslint-plugin-import'
import a11yPlugin from 'eslint-plugin-jsx-a11y'

export default tseslint.config(
    // ===== ESLINT BASE RULES =====
    eslint.configs.recommended,
    
    // ===== TYPESCRIPT-ESLINT CONFIGURATIONS =====
    // Include ALL strict TypeScript rules (includes recommended)
    tseslint.configs.strictTypeChecked,
    tseslint.configs.stylisticTypeChecked,
    
    // ===== ESLINT CORE RULES CUSTOMIZATION =====
    {
        rules: {
            'arrow-parens': ['error', 'as-needed'],
            'no-var': 'error',
            'no-eval': 'error',
            'indent': ['error', 4],
            'quotes': ['error', 'single'],
            'no-console': ["error", { allow: ["warn", "error", "info", "trace", ] }], // Stricter than original
            'space-before-function-paren': ['error', 'never'],
            'padded-blocks': ['error', 'never'],
            'prefer-arrow-callback': ['error', { // Stricter than original
                allowNamedFunctions: true
            }],
            'func-names': ['error', 'never'],
            'no-use-before-define': 'off', // Let typescript-eslint handle this
            'max-len': ['error', 120],
            'object-curly-spacing': ['error', 'always'], // Stricter than original
            'comma-dangle': ['error', 'never'],
            'semi': ['error', 'never'],
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
            'eqeqeq': ['error', 'always']
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
            'import/no-cycle': 'error',
            'import/no-unused-modules': 'error'
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
                tsconfigRootDir: import.meta.dirname,
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
            '@typescript-eslint/prefer-literal-enum-member': 'error'
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

