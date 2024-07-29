# Utilisation de TypeScript avec Vue.js

## Introduction

TypeScript et Vue.js peuvent être utilisés ensemble pour créer des applications web modernes, maintenables et robustes. Cette documentation vous guidera à travers les concepts essentiels et les configurations nécessaires pour tirer parti de TypeScript dans un projet Vue.js.

## Installation et Configuration

### Création d'un projet Vue.js avec TypeScript

La manière la plus simple de démarrer un projet Vue.js avec TypeScript est d'utiliser Vue CLI.

```bash
npm install -g @vue/cli
vue create my-vue-typescript-app
```

Lors de la création du projet, sélectionnez `Manually select features` et choisissez `TypeScript` dans la liste des fonctionnalités.

### Structure du Projet

Après avoir configuré le projet avec Vue CLI, votre structure de projet devrait ressembler à ceci :

```
my-vue-typescript-app/
├── node_modules/
├── public/
├── src/
│   ├── assets/
│   ├── components/
│   │   └── HelloWorld.vue
│   ├── views/
│   │   └── Home.vue
│   ├── App.vue
│   ├── main.ts
│   └── shims-vue.d.ts
├── .gitignore
├── babel.config.js
├── package.json
├── tsconfig.json
└── vue.config.js
```

## Configuration de TypeScript

### `tsconfig.json`

Le fichier `tsconfig.json` contient les options de compilation TypeScript. Un exemple typique pour un projet Vue.js :

```json
{
  "compilerOptions": {
    "target": "esnext",
    "module": "esnext",
    "strict": true,
    "jsx": "preserve",
    "importHelpers": true,
    "moduleResolution": "node",
    "experimentalDecorators": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "types": ["webpack-env", "jest", "node"],
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  },
  "include": ["src/**/*.ts", "src/**/*.tsx", "src/**/*.vue", "tests/**/*.ts", "tests/**/*.tsx"],
  "exclude": ["node_modules"]
}
```

### `shims-vue.d.ts`

Ce fichier permet à TypeScript de comprendre les imports de fichiers `.vue`.

```typescript
declare module '*.vue' {
  import Vue from 'vue';
  export default Vue;
}
```

## Utilisation de TypeScript dans les Composants Vue

### Composant Vue de base en TypeScript

Voici un exemple de composant Vue en TypeScript :

```vue
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator';

@Component
export default class HelloWorld extends Vue {
  @Prop() private msg!: string;
}
</script>

<style scoped>
h1 {
  color: #42b983;
}
</style>
```

### Décorateurs

`vue-property-decorator` est une bibliothèque qui permet d'utiliser les décorateurs pour les propriétés, méthodes, etc.

#### Installation

```bash
npm install vue-property-decorator
```

#### Utilisation

```typescript
import { Vue, Component, Prop } from 'vue-property-decorator';

@Component
export default class MyComponent extends Vue {
  @Prop() private msg!: string;

  private dataProperty: string = 'Hello';

  private created() {
    console.log('Component created');
  }

  private method() {
    console.log('This is a method');
  }
}
```

### Types de Props

Utiliser les types pour les props permet de s'assurer que les bonnes données sont passées aux composants.

```typescript
@Component
export default class MyComponent extends Vue {
  @Prop({ type: String, required: true }) private msg!: string;
  @Prop({ type: Number, default: 0 }) private count!: number;
}
```

### Computed Properties et Watchers

Les propriétés calculées et les watchers peuvent également être typés.

```typescript
@Component
export default class MyComponent extends Vue {
  private firstName: string = 'John';
  private lastName: string = 'Doe';

  get fullName(): string {
    return `${this.firstName} ${this.lastName}`;
  }

  @Watch('firstName')
  onFirstNameChanged(newVal: string, oldVal: string) {
    console.log(`First name changed from ${oldVal} to ${newVal}`);
  }
}
```

## Utilisation de Vuex avec TypeScript

### Configuration de Vuex

Pour utiliser Vuex avec TypeScript, vous devez configurer les types pour les états, les mutations, et les actions.

#### Installation

```bash
npm install vuex
npm install @types/vuex
```

### Typage de l'État

Définissez une interface pour l'état.

```typescript
interface State {
  count: number;
}

const state: State = {
  count: 0
};
```

### Typage des Mutations

Utilisez les types pour les mutations afin de garantir que seules les modifications autorisées sont effectuées.

```typescript
import { MutationTree } from 'vuex';

const mutations: MutationTree<State> = {
  increment(state) {
    state.count++;
  },
  decrement(state) {
    state.count--;
  }
};
```

### Typage des Actions

De même, vous pouvez typer les actions.

```typescript
import { ActionTree } from 'vuex';
import { State } from './state';
import { RootState } from '../index';

const actions: ActionTree<State, RootState> = {
  increment({ commit }) {
    commit('increment');
  },
  decrement({ commit }) {
    commit('decrement');
  }
};
```

### Création du Store

```typescript
import Vue from 'vue';
import Vuex, { StoreOptions } from 'vuex';
import { State } from './state';
import { mutations } from './mutations';
import { actions } from './actions';

Vue.use(Vuex);

const store: StoreOptions<State> = {
  state,
  mutations,
  actions
};

export default new Vuex.Store<State>(store);
```

## Outils et Écosystème

### ESLint et Prettier

Pour garantir la qualité du code, utilisez ESLint et Prettier avec des configurations spécifiques pour TypeScript et Vue.

#### Installation

```bash
npm install --save-dev eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin eslint-plugin-vue
npm install --save-dev prettier eslint-config-prettier eslint-plugin-prettier
```

#### Configuration

Créez un fichier `.eslintrc.js` :

```javascript
module.exports = {
  root: true,
  env: {
    node: true,
  },
  extends: [
    'plugin:vue/essential',
    'eslint:recommended',
    '@vue/typescript/recommended',
    'plugin:prettier/recommended',
  ],
  parserOptions: {
    ecmaVersion: 2020,
  },
  rules: {
    'no-console': process.env.NODE_ENV === 'production' ? 'warn' : 'off',
    'no-debugger': process.env.NODE_ENV === 'production' ? 'warn' : 'off',
  },
};
```

### Webpack

Vue CLI utilise Webpack en interne, donc la configuration manuelle de Webpack n'est généralement pas nécessaire. Cependant, vous pouvez personnaliser la configuration via `vue.config.js`.

### Vue Devtools

Vue Devtools est un outil indispensable pour déboguer les applications Vue.js. Installez l'extension Vue Devtools pour votre navigateur.

## Bonnes Pratiques

### Typage stricte

Utilisez les types stricts dans TypeScript pour tirer le meilleur parti du typage statique et améliorer la maintenabilité de votre code.

### Utiliser les décorateurs

Les décorateurs permettent une meilleure organisation et lisibilité du code, surtout pour les propriétés, méthodes et événements.

### Diviser le code en composants

Divisez votre application en petits composants réutilisables pour améliorer la modularité et la maintenabilité.

### Documentation

Documentez vos composants et fonctions pour améliorer la compréhension et faciliter la collaboration.

## Ressources supplémentaires

- [Documentation officielle de TypeScript](https://www.typescriptlang.org/docs/)
- [Documentation officielle de Vue.js](https://vuejs.org/v2/guide/typescript.html)
- [Vue Class Component](https://github.com/vuejs/vue-class-component)
- [Vue Property Decorator](https://github.com/kaorun343/vue-property-decorator)

---

Cette documentation couvre les concepts essentiels pour utiliser TypeScript avec Vue.js. Pour approfondir vos connaissances, consultez les ressources supplémentaires et expérimentez avec des exemples pratiques.
