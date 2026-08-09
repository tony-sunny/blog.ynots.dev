+++
title = 'TypeScript Configuration'
date = 2024-08-04T16:45:12+05:30
draft = false
tags = ['typescript']
+++

There are a lot of options to tweak TypeScript behavior. \
These are the options that I have found very useful.
<!--more-->

The below config is for node js projects.

```json
{
  "compilerOptions": {
    "declaration": true,
    "declarationMap": true,
    "esModuleInterop": true,
    "module": "nodeNext",
    "noImplicitReturns": true,
    "noUncheckedIndexedAccess": true,
    "noUnusedParameters": true,
    "noUnusedLocals": true,
    "strict": true,
    "verbatimModuleSyntax": true
  }
}
```

Refer [tsconfig](https://www.typescriptlang.org/tsconfig/) for more details.

For [target](https://www.typescriptlang.org/tsconfig/#target), refer below github repo
for suggested bases for each node js version. \
https://github.com/tsconfig/bases/tree/main/bases

## Changes in TypeScript 7

TypeScript 7 — the native Go port, released July 2026 — makes two of the options above unnecessary.

`strict` is now `true` by default, so you only need the flag if you prefer stating it explicitly.

`esModuleInterop` and `allowSyntheticDefaultImports` can no longer be set to `false`. The behaviour is always on, so the flag no longer carries any meaning.


That leaves:

```json
{
  "compilerOptions": {
    "declaration": true,
    "declarationMap": true,
    "module": "nodeNext",
    "noImplicitReturns": true,
    "noUncheckedIndexedAccess": true,
    "noUnusedParameters": true,
    "noUnusedLocals": true,
    "verbatimModuleSyntax": true
  }
}
```

Refer [the TypeScript 7 announcement](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/) for more details.
