# Documentation Complète sur TypeScript

## Introduction

TypeScript est un langage open-source développé par Microsoft. C'est un sur-ensemble de JavaScript qui ajoute des types statiques facultatifs, ce qui permet de détecter les erreurs au moment de la compilation et de rendre le développement JavaScript plus robuste et maintenable.

## Installation et Configuration

### Installation

Pour installer TypeScript globalement sur votre machine, utilisez npm :

```bash
npm install -g typescript
```

### Vérifier l'installation

Pour vérifier que TypeScript est bien installé, utilisez la commande suivante :

```bash
tsc --version
```

### Configuration avec `tsconfig.json`

TypeScript utilise un fichier de configuration nommé `tsconfig.json` pour définir les options de compilation et les fichiers à inclure dans le projet.

#### Exemple de `tsconfig.json`

```json
{
  "compilerOptions": {
    "target": "es6",
    "module": "commonjs",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "outDir": "./dist",
    "rootDir": "./src"
  },
  "include": ["src"],
  "exclude": ["node_modules", "**/*.spec.ts"]
}
```

## Syntaxe de Base

### Types de base

TypeScript étend JavaScript en ajoutant des types de base comme `number`, `string`, `boolean`, etc.

```typescript
let isDone: boolean = false;
let decimal: number = 6;
let color: string = "blue";
let list: number[] = [1, 2, 3];
let x: [string, number] = ["hello", 10]; // Tuple
```

### Enum

Les énumérations permettent de définir un ensemble de valeurs nommées.

```typescript
enum Color {
  Red,
  Green,
  Blue
}
let c: Color = Color.Green;
```

### Any

Le type `any` permet de désactiver le typage pour une variable, utile pour intégrer des bibliothèques JavaScript non typées.

```typescript
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false;
```

### Void

Le type `void` est utilisé pour les fonctions qui ne retournent aucune valeur.

```typescript
function warnUser(): void {
  console.log("This is my warning message");
}
```

## Fonctions

### Déclaration de fonction

TypeScript permet de définir des types pour les paramètres et le retour des fonctions.

```typescript
function add(x: number, y: number): number {
  return x + y;
}
```

### Fonctions anonymes

```typescript
let myAdd = function(x: number, y: number): number {
  return x + y;
};
```

### Type de fonction

On peut définir explicitement le type d'une fonction.

```typescript
let myAdd: (x: number, y: number) => number =
  function(x: number, y: number): number { return x + y; };
```

### Paramètres optionnels et par défaut

TypeScript permet de définir des paramètres optionnels et des valeurs par défaut.

```typescript
function buildName(firstName: string, lastName?: string): string {
  if (lastName)
    return firstName + " " + lastName;
  else
    return firstName;
}

function buildNameDefault(firstName: string, lastName = "Smith"): string {
  return firstName + " " + lastName;
}
```

## Interfaces

### Déclaration

Les interfaces définissent la structure des objets.

```typescript
interface LabelledValue {
  label: string;
}

function printLabel(labelledObj: LabelledValue) {
  console.log(labelledObj.label);
}

let myObj = {size: 10, label: "Size 10 Object"};
printLabel(myObj);
```

### Propriétés optionnelles

Les propriétés d'une interface peuvent être optionnelles.

```typescript
interface SquareConfig {
  color?: string;
  width?: number;
}

function createSquare(config: SquareConfig): {color: string; area: number} {
  let newSquare = {color: "white", area: 100};
  if (config.color) {
    newSquare.color = config.color;
  }
  if (config.width) {
    newSquare.area = config.width * config.width;
  }
  return newSquare;
}
```

### Propriétés en lecture seule

Les propriétés en lecture seule ne peuvent être modifiées après leur initialisation.

```typescript
interface Point {
  readonly x: number;
  readonly y: number;
}

let p1: Point = { x: 10, y: 20 };
// p1.x = 5; // Error: cannot assign to 'x' because it is a read-only property
```

## Classes

### Déclaration

Les classes en TypeScript sont similaires à celles de JavaScript avec des ajouts pour le typage et les modificateurs d'accès.

```typescript
class Greeter {
  greeting: string;
  constructor(message: string) {
    this.greeting = message;
  }
  greet() {
    return "Hello, " + this.greeting;
  }
}

let greeter = new Greeter("world");
```

### Héritage

Les classes peuvent étendre d'autres classes.

```typescript
class Animal {
  move(distanceInMeters: number = 0) {
    console.log(`Animal moved ${distanceInMeters}m.`);
  }
}

class Dog extends Animal {
  bark() {
    console.log('Woof! Woof!');
  }
}

const dog = new Dog();
dog.bark();
dog.move(10);
dog.bark();
```

### Modificateurs d'accès

TypeScript supporte les modificateurs d'accès `public`, `private`, et `protected`.

```typescript
class Animal {
  private name: string;
  public constructor(theName: string) { this.name = theName; }
  public move(distanceInMeters: number) {
    console.log(`${this.name} moved ${distanceInMeters}m.`);
  }
}

class Rhino extends Animal {
  constructor() { super("Rhino"); }
}

class Employee {
  private name: string;
  constructor(theName: string) { this.name = theName; }
}

let animal = new Animal("Goat");
let rhino = new Rhino();
let employee = new Employee("Bob");

animal = rhino;
// animal = employee; // Error: 'Animal' and 'Employee' are not compatible
```

### Classes abstraites

Les classes abstraites ne peuvent pas être instanciées directement. Elles servent de modèles pour d'autres classes.

```typescript
abstract class Department {
  constructor(public name: string) {
  }

  printName(): void {
    console.log("Department name: " + this.name);
  }

  abstract printMeeting(): void; // Must be implemented in derived classes
}

class AccountingDepartment extends Department {
  constructor() {
    super("Accounting and Auditing");
  }

  printMeeting(): void {
    console.log("The Accounting Department meets each Monday at 10am.");
  }

  generateReports(): void {
    console.log("Generating accounting reports...");
  }
}

let department: Department; // ok to create a reference to an abstract type
department = new AccountingDepartment(); // ok to create and assign a non-abstract subclass
department.printName();
department.printMeeting();
// department.generateReports(); // Error: method doesn't exist on declared abstract type
```

### Propriétés statiques

Les propriétés et méthodes statiques appartiennent à la classe elle-même plutôt qu'aux instances de la classe.

```typescript
class Grid {
  static origin = { x: 0, y: 0 };
  calculateDistanceFromOrigin(point: { x: number; y: number; }) {
    let xDist = (point.x - Grid.origin.x);
    let yDist = (point.y - Grid.origin.y);
    return Math.sqrt(xDist * xDist + yDist * yDist) / this.scale;
  }
  constructor (public scale: number) { }
}

let grid1 = new Grid(1.0);  // 1x scale
let grid2 = new Grid(5.0);  // 5x scale

console.log(grid1.calculateDistanceFromOrigin({ x: 10, y: 10 }));
console.log(grid2.calculateDistanceFromOrigin({ x: 10, y: 10 }));
```

### Classes génériques

Les classes génériques permettent de créer des composants réutilisables qui fonctionnent avec différents types de données.

```typescript
class GenericNumber<T> {
  zeroValue: T;
  add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };

let stringNumeric = new GenericNumber<string>();
stringNumeric.zeroValue = "";
stringNumeric.add = function(x, y) { return x + y; };

console.log(stringNumeric.add(stringNumeric.zeroValue, "test"));
```

### Décorateurs

Les décorateurs sont une fonctionnalité expérimentale de TypeScript qui permettent d'ajouter des métadonnées aux classes et aux membres de classe.

```typescript
function sealed(constructor: Function) {
  Object.seal(constructor);
  Object.seal(constructor.prototype);
}

@sealed
class Greeter {
  greeting: string;
  constructor(message: string) {
    this.greeting = message;
  }
  greet() {
    return "Hello, " + this.greeting;
  }
}
```
### Mixins

Les mixins permettent de combiner plusieurs classes en une seule. Cela permet d'ajouter des fonctionnalités à une classe sans utiliser l'héritage classique.

#### Exemple de Mixins

```typescript
class Disposable {
  isDisposed: boolean = false;
  dispose() {
    this.isDisposed = true;
  }
}

class Activatable {
  isActive: boolean = false;
  activate() {
    this.isActive = true;
  }
  deactivate() {
    this.isActive = false;
  }
}

class SmartObject implements Disposable, Activatable {
  isDisposed: boolean = false;
  dispose: () => void;
  isActive: boolean = false;
  activate: () => void;
  deactivate: () => void;

  constructor() {
    setInterval(() => console.log(this.isActive + " : " + this.isDisposed), 500);
  }
}

applyMixins(SmartObject, [Disposable, Activatable]);

function applyMixins(derivedCtor: any, baseCtors: any[]) {
  baseCtors.forEach(baseCtor => {
    Object.getOwnPropertyNames(baseCtor.prototype).forEach(name => {
      derivedCtor.prototype[name] = baseCtor.prototype[name];
    });
  });
}
```

Dans cet exemple, `SmartObject` combine les fonctionnalités de `Disposable` et `Activatable`.

## Modules

Les modules sont un moyen de structurer le code en plusieurs fichiers et de gérer les dépendances entre eux. TypeScript suit les standards ECMAScript pour les modules.

### Export

Pour exporter une fonctionnalité d'un module, utilisez le mot-clé `export`.

```typescript
export interface StringValidator {
  isAcceptable(s: string): boolean;
}

export const numberRegexp = /^[0-9]+$/;

export class ZipCodeValidator implements StringValidator {
  isAcceptable(s: string) {
    return s.length === 5 && numberRegexp.test(s);
  }
}
```

### Import

Pour importer une fonctionnalité d'un autre module, utilisez le mot-clé `import`.

```typescript
import { StringValidator } from "./Validation";

let myValidator: StringValidator = ...;
```

### Export par défaut

Vous pouvez exporter une fonctionnalité par défaut, ce qui permet de l'importer sans accolades.

```typescript
export default class ZipCodeValidator {
  isAcceptable(s: string) {
    return s.length === 5 && numberRegexp.test(s);
  }
}
```

### Importation du défaut

```typescript
import ZipCodeValidator from "./ZipCodeValidator";

let myValidator = new ZipCodeValidator();
```

## Utilisation Avancée

### Types Conditionnels

Les types conditionnels permettent de définir des types basés sur une condition. Ils sont utiles pour créer des types plus flexibles et dynamiques.

```typescript
type MessageOf<T> = T extends { message: unknown } ? T["message"] : never;

interface Email {
  message: string;
}

interface Dog {
  bark(): void;
}

type EmailMessageContents = MessageOf<Email>; // string
type DogMessageContents = MessageOf<Dog>; // never
```

### Types Mappés

Les types mappés permettent de transformer les propriétés d'un type existant pour créer un nouveau type.

```typescript
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};

interface Person {
  name: string;
  age: number;
}

const readonlyPerson: Readonly<Person> = {
  name: "John",
  age: 30
};

// readonlyPerson.age = 25; // Error: cannot assign to 'age' because it is a read-only property
```

### Types Unions et Intersections

Les types unions permettent de définir un type qui peut être l'un ou l'autre de plusieurs types, tandis que les types intersections combinent plusieurs types en un seul.

```typescript
// Union Type
let value: string | number;
value = "hello"; // OK
value = 123; // OK

// Intersection Type
interface Colorful {
  color: string;
}

interface Circle {
  radius: number;
}

type ColorfulCircle = Colorful & Circle;

const colorfulCircle: ColorfulCircle = {
  color: "red",
  radius: 42
};
```

## Utilisation des Modules Externes

### Utilisation d'une Bibliothèque Externe

Pour utiliser une bibliothèque JavaScript tierce avec TypeScript, vous devez installer les types de cette bibliothèque.

```bash
npm install lodash
npm install @types/lodash
```

Ensuite, vous pouvez importer et utiliser la bibliothèque dans votre fichier TypeScript.

```typescript
import _ from "lodash";

let array = [1, 2, 3, 4];
let reversedArray = _.reverse(array);
console.log(reversedArray); // [4, 3, 2, 1]
```

## TypeScript et JSX

TypeScript supporte JSX (une syntaxe de balisage utilisée avec React). Les fichiers qui contiennent JSX utilisent l'extension `.tsx`.

```typescript
import React from 'react';

interface AppProps {
  message: string;
}

const App: React.FC<AppProps> = ({ message }) => {
  return <div>{message}</div>;
};

export default App;
```

## Compilation et Build

### Compilation simple

Pour compiler un fichier TypeScript en JavaScript, utilisez la commande suivante :

```bash
tsc file.ts
```

### Compilation avec `tsconfig.json`

Pour compiler un projet entier basé sur les options définies dans `tsconfig.json` :

```bash
tsc
```

## Outils et Écosystème

### ESLint

ESLint est un outil populaire pour analyser et corriger les erreurs dans le code TypeScript.

```bash
npm install eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin --save-dev
```

### Exemple de configuration ESLint

```json
{
  "parser": "@typescript-eslint/parser",
  "plugins": ["@typescript-eslint"],
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended"
  ],
  "rules": {
    // Ajouter vos règles personnalisées ici
  }
}
```

### Webpack

Webpack est un bundler de modules qui peut être utilisé pour compiler et empaqueter les fichiers TypeScript.

```bash
npm install --save-dev webpack webpack-cli ts-loader
```

### Exemple de configuration Webpack

```javascript
// webpack.config.js
const path = require('path');

module.exports = {
  entry: './src/index.ts',
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: 'ts-loader',
        exclude: /node_modules/,
      },
    ],
  },
  resolve: {
    extensions: ['.tsx', '.ts', '.js'],
  },
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
};
```

## Bonnes Pratiques

### Utiliser les types stricts

Activez les options strictes dans `tsconfig.json` pour tirer le meilleur parti de TypeScript.

```json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "strictPropertyInitialization": true,
    "noImplicitThis": true,
    "alwaysStrict": true
  }
}
```

### Typage explicite

Déclarez explicitement les types pour les variables, les paramètres de fonction et les valeurs de retour pour rendre le code plus lisible et éviter les erreurs.

```typescript
function add(a: number, b: number): number {
  return a + b;
}
```

### Utiliser `readonly` pour les propriétés immuables

Marquez les propriétés qui ne doivent pas être modifiées comme `readonly`.

```typescript
interface Person {
  readonly name: string;
  readonly age: number;
}
```

### Éviter `any`

Évitez d'utiliser le type `any` autant que possible, car il annule les avantages de TypeScript en matière de vérification de types.

```typescript
function processData(data: any): void {
  // Essayez d'éviter d'utiliser 'any'
}
```

## Ressources supplémentaires

- [Documentation officielle de TypeScript](https://www.typescriptlang.org/docs/)
- [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/)
- [Handbook TypeScript](https://www.typescriptlang.org/docs/handbook/intro.html)
- [Playground TypeScript](https://www.typescriptlang.org/play)

---

Cette documentation couvre une large gamme de fonctionnalités de TypeScript, des bases aux concepts avancés. N'hésitez pas à explorer davantage et à poser des questions spécifiques pour approfondir vos connaissances.
