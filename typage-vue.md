## Introduction à TypeScript avec Vue.js

TypeScript est un sur-ensemble de JavaScript qui ajoute des types statiques. Cela signifie que vous pouvez définir les types de données de vos variables et fonctions, ce qui permet de détecter les erreurs plus tôt lors du développement. Avec Vue.js, TypeScript aide à rendre les composants plus robustes et maintenables.

### Installation et Configuration de TypeScript avec Vue.js

Avant de commencer, assurez-vous d'avoir Node.js et npm installés sur votre machine. Vous pouvez créer un projet Vue.js avec TypeScript en utilisant Vue CLI :

```bash
npm install -g @vue/cli
vue create my-vue-app
```

Pendant la configuration du projet, sélectionnez "TypeScript" pour ajouter le support TypeScript.

### Les Variables `ref` en TypeScript

Dans Vue.js, la fonction `ref` est utilisée pour créer des références réactives. En TypeScript, il est important de typer correctement ces références.

#### Importer les Fonctionnalités Nécessaires

```typescript
import { ref } from 'vue';
```

#### Typage d'une Référence

Lorsque vous utilisez `ref`, vous pouvez définir explicitement le type de la variable. Cela se fait en utilisant la syntaxe générique.

```typescript
const count = ref<number>(0);
```

Ici, `count` est une référence réactive de type `number`.

#### Typage des Objets

Pour les objets, vous pouvez créer une interface et l'utiliser comme type.

```typescript
interface User {
  name: string;
  age: number;
}

const user = ref<User>({ name: 'Alice', age: 25 });
```

#### Typage des Références DOM

Pour référencer des éléments DOM, utilisez `ref` avec `null` comme valeur initiale.

```typescript
import { ref, onMounted } from 'vue';

const inputRef = ref<HTMLInputElement | null>(null);

onMounted(() => {
  if (inputRef.value) {
    inputRef.value.focus();
  }
});
```

### Typage des Fonctions en TypeScript

TypeScript permet également de typer les fonctions, y compris leurs paramètres et leur valeur de retour.

#### Fonctions Simples

Pour typer une fonction simple, vous définissez les types des paramètres et le type de retour.

```typescript
function add(a: number, b: number): number {
  return a + b;
}
```

Ici, `add` prend deux arguments de type `number` et retourne un `number`.

#### Fonctions Asynchrones

Les fonctions asynchrones retournent une `Promise`. Vous devez donc typer le retour en conséquence.

```typescript
async function fetchData(url: string): Promise<string> {
  const response = await fetch(url);
  return response.text();
}
```

#### Fonctions avec Objets en Paramètre

Pour les fonctions qui acceptent des objets en paramètre, utilisez des interfaces pour définir les types.

```typescript
interface User {
  name: string;
  age: number;
}

function greet(user: User): string {
  return `Hello, ${user.name}`;
}
```

### Exemple Complet

Voici un exemple complet d'un composant Vue avec TypeScript, utilisant des références et des fonctions typées.

#### `HelloWorld.vue`

```typescript
<template>
  <div>
    <p>{{ message }}</p>
    <input ref="inputRef" v-model="user.name" />
    <button @click="updateMessage">Update Message</button>
  </div>
</template>

<script lang="ts">
import { ref, defineComponent, onMounted } from 'vue';

interface User {
  name: string;
  age: number;
}

export default defineComponent({
  name: 'HelloWorld',
  setup() {
    const message = ref<string>('Hello, World!');
    const user = ref<User>({ name: 'Alice', age: 25 });
    const inputRef = ref<HTMLInputElement | null>(null);

    const updateMessage = (): void => {
      message.value = `Hello, ${user.value.name}`;
    };

    onMounted(() => {
      if (inputRef.value) {
        inputRef.value.focus();
      }
    });

    return {
      message,
      user,
      inputRef,
      updateMessage
    };
  }
});
</script>
```

### Conclusion

Le typage des variables et des fonctions en TypeScript améliore la robustesse et la maintenabilité de votre code Vue.js. En spécifiant les types, vous bénéficiez d'une meilleure autocomplétion et de vérifications de type à la compilation, ce qui réduit les erreurs potentielles. En suivant les principes décrits dans ce cours, vous pouvez créer des applications Vue.js plus fiables et plus faciles à
