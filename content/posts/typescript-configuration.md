+++
title = 'Typescript Configuration'
date = 2024-08-04T16:45:12+05:30
draft = false
+++

There are a lot of options to tweak typescript behavior. \
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
for suggested bases for each node js version.
https://github.com/tsconfig/bases/tree/main/bases
