---

## Angular 17 : Révolution et Performance dans le Développement Web

### Introduction
Angular 17 marque un tournant majeur pour le framework, apportant des innovations significatives axées sur la performance, l'expérience développeur et une simplification du code. Cette version ne se contente pas d'améliorer l'existant ; elle introduit de nouvelles primitives pour une réactivité plus granulaire et une architecture plus moderne. Découvrons ensemble les fonctionnalités phares qui propulsent Angular vers l'avenir du développement web.

### Des Blocs de Contrôle Modernes pour des Templates Plus Efficaces
L'une des nouveautés les plus visibles est l'introduction d'une nouvelle syntaxe de contrôle de flux dans les templates. Fini les `*ngIf`, `*ngFor` et `*ngSwitch` basés sur des directives structurelles ; Angular 17 propose désormais des blocs natifs tels que `@if`, `@for` et `@switch`.

Ces nouveaux blocs offrent plusieurs avantages :
*   **Lisibilité accrue** : La syntaxe est plus proche du JavaScript standard, rendant le code plus intuitif.
*   **Performance améliorée** : Ils sont compilés directement en instructions JavaScript optimisées, sans les surcharges des directives structurelles, ce qui se traduit par des bundles plus petits et un rendu plus rapide.
*   **Aucun changement lors de l'exécution** : Contrairement aux anciennes directives, ces blocs ne nécessitent pas de micro-syntaxe interprétée à l'exécution, réduisant la complexité.

Par exemple, un `@if` permet d'afficher ou de masquer des éléments conditionnellement, tandis que `@for` simplifie l'itération sur des collections avec une gestion intégrée des éléments vides et des identifiants de suivi (`track`) pour des mises à jour DOM optimales.

### Optimisation du Chargement avec le Bloc `@defer`
Le bloc `@defer` est une autre innovation majeure axée sur la performance. Il permet de différer le chargement et le rendu de parties de votre application jusqu'à ce qu'elles soient réellement nécessaires, améliorant ainsi considérablement le temps de chargement initial et l'expérience utilisateur.

Imaginez une section de votre page qui n'est visible qu'après un défilement ou une interaction. Avec `@defer`, vous pouvez spécifier quand cette section doit être chargée :
*   `on idle` : Lorsque le navigateur est inactif.
*   `on viewport` : Lorsque l'élément entre dans la zone visible de l'utilisateur.
*   `on hover` : Au survol de l'élément.
*   `on interaction` : Au clic ou à une autre interaction.

Le bloc `@defer` peut également inclure des blocs `placeholder` pour afficher un contenu temporaire, `loading` pour indiquer un chargement en cours, et `error` pour gérer les échecs de chargement. C'est un outil puissant pour le "lazy loading" (chargement paresseux) ciblé.

### SSR et Hydratation : Des Performances Inégalées par Défaut
Angular 17 fait du Server-Side Rendering (SSR) et de l'hydratation des éléments par défaut pour les nouveaux projets.
*   **SSR** : Le rendu côté serveur génère le HTML de votre application sur le serveur avant de l'envoyer au client. Cela améliore le référencement (SEO) et offre un affichage initial plus rapide (First Contentful Paint), car le contenu est immédiatement visible.
*   **Hydratation** : C'est le processus qui permet de réutiliser le DOM généré par le SSR et d'y attacher les fonctionnalités JavaScript côté client, évitant ainsi le "flickering" (scintillement) et offrant une transition fluide vers une application pleinement interactive.

Ces améliorations, combinées à l'intégration d'outils de compilation plus rapides comme Vite et esbuild, permettent à Angular 17 de produire des applications plus petites, plus rapides et plus efficaces.

### L'Écosystème Angular Modernisé
Angular 17 poursuit la poussée vers les composants "Standalone" (autonomes), qui sont désormais le choix par défaut pour les nouveaux projets. Ils simplifient l'architecture en permettant aux composants d'importer directement leurs dépendances, réduisant la dépendance aux modules NgModules.

Les Signals, introduits en Angular 16 et stabilisés en 17, offrent une nouvelle approche pour la gestion de l'état réactif, apportant une réactivité plus granulaire et simplifiée à l'écosystème.

Enfin, Angular 17 propose une expérience développeur améliorée avec des diagnostics plus clairs, des messages d'erreur plus précis, et une chaîne d'outils plus rapide, facilitant la création et la maintenance d'applications robustes.

### Conclusion
Angular 17 est une version transformative qui met l'accent sur la performance, la simplicité et l'expérience développeur. Avec ses nouveaux blocs de contrôle, le chargement différé, le SSR/hydratation par défaut, et l'intégration des Standalone Components et Signals, le framework réaffirme sa position comme un choix de premier ordre pour le développement web moderne et performant.

### Sources
*   [Angular 17: The Evolution of Web Development](https://dev.to/this-is-angular/angular-17-the-evolution-of-web-development-1h13)
*   [Angular v17: SSR, Hydratation, Performance and More (Part 2)](https://dev.to/this-is-angular/angular-v17-ssr-hydratation-performance-and-more-part-2-4k5a)

---

## Comprendre et Utiliser les Composants Standalone dans Angular

### Introduction
L'écosystème Angular évolue constamment, et l'une des avancées les plus significatives de ces dernières années est l'introduction des composants Standalone (autonomes). Ces composants changent la manière traditionnelle de structurer les applications Angular, offrant une approche plus simple, plus modulaire et plus performante. Adopter les Standalone Components, c'est embrasser une vision plus moderne et épurée du développement Angular.

### Qu'est-ce qu'un Composant Standalone ?
Traditionnellement, les composants, directives et pipes Angular devaient être déclarés au sein d'un `NgModule` pour être utilisables. Les composants Standalone brisent cette dépendance : ils peuvent être utilisés sans nécessiter l'enveloppement dans un module Angular.

Pour transformer un composant existant en composant Standalone, ou en créer un nouveau, il suffit d'ajouter la propriété `standalone: true` à son décorateur `@Component` :

```typescript
@Component({
  standalone: true,
  selector: 'app-mon-composant',
  templateUrl: './mon-composant.component.html',
  styleUrls: ['./mon-composant.component.css'],
  imports: [CommonModule, MonAutreComposantStandalone] // Importe directement les dépendances
})
export class MonComposantComponent {
  // ...
}
```

La principale différence réside dans l'array `imports` directement au sein du décorateur `@Component`. Au lieu de dépendre d'un `NgModule` pour déclarer et importer des dépendances (comme `CommonModule`, d'autres composants, directives, ou pipes), un composant Standalone importe directement ce dont il a besoin.

### Bootstrapping d'une Application Standalone
Avec les composants Standalone, la manière de démarrer votre application change également. Fini l'appel à `platformBrowserDynamic().bootstrapModule(AppModule)`. Désormais, vous utilisez la fonction `bootstrapApplication` et lui passez directement votre composant racine Standalone :

```typescript
import { bootstrapApplication } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import { provideRouter } from '@angular/router';
import { routes } from './app.routes';
import { provideHttpClient } from '@angular/common/http';

bootstrapApplication(AppComponent, {
  providers: [
    provideRouter(routes),
    provideHttpClient()
  ]
}).catch(err => console.error(err));
```

Dans cette approche, les services globaux (comme ceux pour le routage ou les requêtes HTTP) sont configurés via le second argument de `bootstrapApplication`, l'objet `ApplicationConfig`, en utilisant des fonctions de configuration comme `provideRouter` et `provideHttpClient`.

### Avantages des Composants Standalone
Les composants Standalone apportent de nombreux bénéfices :

*   **Simplicité et clarté** : Le modèle mental est simplifié. Vous n'avez plus à vous soucier de savoir où déclarer un composant ou quels modules importer dans votre `NgModule`. Tout est géré localement par le composant.
*   **Meilleure performance et taille de bundle** : La structure Standalone améliore le "tree-shaking", permettant aux outils de compilation d'éliminer plus efficacement le code inutilisé. Cela réduit la taille finale de votre application et améliore les performances de chargement.
*   **Expérience développeur améliorée** : Moins de boilerplate, plus de focus sur le composant lui-même. Les outils de migration automatique facilitent le passage vers cette architecture.
*   **Meilleure modularité** : Chaque composant gère ses propres dépendances, ce qui rend les composants plus autonomes et réutilisables dans différentes parties de l'application ou même dans d'autres projets.
*   **Migration facilitée** : Angular fournit des schémas (`ng generate @angular/core:standalone`) pour aider à la migration progressive de vos applications existantes vers l'architecture Standalone.

### Gérer les Dépendances et les Providers
Même si les NgModules sont moins présents, il est crucial de bien organiser les imports de vos composants Standalone. Pour les imports récurrents, vous pouvez créer un composant Standalone "fourre-tout" qui exporte un ensemble de modules (`CommonModule`, `FormsModule`, etc.) pour les réutiliser facilement.

Pour les services, la fonction `inject()` devient un moyen moderne et flexible de récupérer des dépendances, même en dehors des constructeurs. Les providers peuvent être configurés au niveau de l'application (via `bootstrapApplication`), au niveau d'un composant (`providers` array dans `@Component`), ou via des fonctions `provide*` spécifiques comme `provideRouter`.

### Conclusion
Les composants Standalone représentent une évolution majeure pour Angular, offrant une architecture plus légère, plus simple et plus performante. En simplifiant la gestion des dépendances et en rendant les composants véritablement autonomes, ils facilitent le développement d'applications Angular modernes et maintenables. Adopter cette approche, c'est choisir l'efficacité et la clarté pour vos projets.

### Sources
*   [Angular Standalone Components Architecture](https://blog.angular-university.io/angular-standalone-components-architecture/)

---

## Gérer l'État Réactif avec les Signals Angular

### Introduction
Dans le monde du développement web, la gestion de l'état d'une application est un défi constant. Angular, avec ses innovations continues, introduit les Signals : une nouvelle primitive réactive conçue pour simplifier la gestion de l'état et améliorer la réactivité de vos applications. Les Signals offrent une approche plus directe et plus performante pour suivre les changements de données et mettre à jour l'interface utilisateur.

### Qu'est-ce qu'un Signal ?
Un Signal est une valeur enveloppée qui notifie les consommateurs lorsqu'elle change. C'est un concept similaire à celui d'un Observable pour les cas plus simples et synchrones, mais avec une granularité et une simplicité d'utilisation accrues. Un Signal est toujours lu via une fonction, ce qui permet à Angular de suivre précisément où la valeur est utilisée et de ne mettre à jour que les parties concernées de l'interface utilisateur.

Pour créer un Signal, vous utilisez la fonction `signal()` :

```typescript
import { signal } from '@angular/core';

const compteur = signal(0); // Crée un signal avec une valeur initiale de 0
console.log(compteur()); // Pour lire la valeur : 0
```

### Modifier un Signal
Un Signal est par défaut en lecture seule. Pour modifier sa valeur, vous utilisez des méthodes spécifiques :
*   `set(newValue)` : Remplace la valeur actuelle par une nouvelle valeur.
*   `update(updaterFn)` : Met à jour la valeur en fonction de sa valeur précédente. Utile pour les opérations comme l'incrémentation.
*   `mutate(mutatorFn)` : Permet de modifier directement un objet stocké dans le signal (utile pour les objets mutables comme les tableaux ou les objets JavaScript).

Exemples :
```typescript
compteur.set(5); // Le compteur devient 5
compteur.update(valeurActuelle => valeurActuelle + 1); // Le compteur devient 6
const monTableau = signal([1, 2, 3]);
monTableau.mutate(arr => arr.push(4)); // Le tableau devient [1, 2, 3, 4]
```

### Valeurs Dérivées avec `computed()`
Souvent, vous avez besoin d'une valeur qui dépend d'un ou plusieurs autres Signals. C'est là qu'intervient la fonction `computed()`. Elle crée un Signal qui recalcule sa valeur automatiquement lorsque les Signals dont il dépend changent. Les valeurs `computed` sont également mises en cache (mémoisées) pour améliorer les performances.

```typescript
import { signal, computed } from '@angular/core';

const prixUnitaire = signal(10);
const quantite = signal(2);

const prixTotal = computed(() => prixUnitaire() * quantite()); // dépend de prixUnitaire et quantite
console.log(prixTotal()); // 20

quantite.set(3);
console.log(prixTotal()); // 30 (automatiquement recalculé)
```

### Effets Secondaires avec `effect()`
La fonction `effect()` est utilisée pour exécuter des effets secondaires en réaction aux changements de Signals. Cela inclut des opérations comme la journalisation, la manipulation directe du DOM, ou l'interaction avec des bibliothèques tierces. Il est important de noter que les `effect()` ne sont pas censés modifier l'état de votre application, mais plutôt réagir à ses changements.

```typescript
import { signal, effect } from '@angular/core';

const utilisateurConnecte = signal(false);

effect(() => {
  console.log(`L'utilisateur est maintenant connecté : ${utilisateurConnecte()}`);
});

utilisateurConnecte.set(true); // L'effet s'exécute et logge la nouvelle valeur
```
Les effets doivent être utilisés avec prudence, car ils peuvent introduire des boucles infinies ou des comportements inattendus s'ils sont mal gérés.

### Signals et Composants Angular
Les Signals s'intègrent parfaitement aux templates Angular. Lorsque vous utilisez un Signal dans un template, Angular détecte automatiquement ses changements et met à jour uniquement la partie du DOM affectée. Cela peut simplifier la stratégie de détection de changements, notamment en réduisant le besoin de `ChangeDetectionStrategy.OnPush`.

Dans le code TypeScript d'un composant, vous devez appeler la fonction du Signal (`compteur()`) pour lire sa valeur.

### Signals vs. RxJS : Complémentarité, pas Remplacement
Il est crucial de comprendre que les Signals ne sont pas destinés à remplacer RxJS. Ils sont complémentaires.
*   **Signals** sont idéaux pour la gestion de l'état local, synchrone et granulaire. Ils excellent dans les scénarios où vous avez besoin d'une réactivité simple et directe.
*   **RxJS** reste indispensable pour les flux de données complexes, asynchrones, les opérations sur le temps, les annulations, et la composition de multiples sources de données.

Les Signals peuvent être utilisés à l'intérieur d'Observables RxJS et vice-versa, permettant de combiner le meilleur des deux mondes.

### Conclusion
Les Signals apportent une nouvelle dimension à la gestion de l'état dans Angular, offrant une approche plus simple, plus performante et plus intuitive pour les scénarios de réactivité courants. En comprenant comment utiliser `signal()`, `computed()` et `effect()`, les développeurs Angular peuvent créer des applications plus réactives et maintenables, tout en continuant à exploiter la puissance de RxJS pour les besoins plus complexes.

### Sources
*   [Angular Signals Guide](https://blog.angular-university.io/angular-signals-guide/)

---

## La Communication entre Composants Angular : Maîtriser Input, Output, ViewChild et plus

### Introduction
Dans une application Angular, les composants sont les blocs de construction fondamentaux. Pour construire des interfaces utilisateur dynamiques et interactives, ces composants doivent souvent échanger des données et des événements entre eux. Comprendre les différentes stratégies de communication est essentiel pour architecturer des applications robustes et maintenables. Cet article explore les méthodes clés de communication, du simple partage de propriétés aux solutions plus avancées pour les composants non liés.

### 1. Communication Parent-Enfant : `@Input()`
La méthode la plus courante pour qu'un composant parent transmette des données à un composant enfant est via la propriété `@Input()`. L'enfant déclare une propriété comme un `@Input()`, et le parent y lie une valeur à l'aide de la liaison de propriété (property binding).

**Comment ça marche :**
1.  **Dans le composant enfant** :
    ```typescript
    import { Component, Input } from '@angular/core';

    @Component({
      selector: 'app-enfant',
      template: '<p>Message de l\'enfant : {{ message }}</p>'
    })
    export class EnfantComponent {
      @Input() message: string = ''; // Déclare la propriété 'message' comme une entrée
    }
    ```
2.  **Dans le composant parent** :
    ```html
    <app-enfant [message]="'Bonjour de la part du parent !'"></app-enfant>
    ```
    Le parent peut aussi lier une propriété de sa propre classe : `<app-enfant [message]="parentMessage"></app-enfant>`.

Vous pouvez également rendre un `@Input()` obligatoire avec `@Input({ required: true })` ou lui donner un alias pour une meilleure lisibilité dans le template du parent : `@Input('monAlias') message: string;`.

### 2. Communication Enfant-Parent : `@Output()` et `EventEmitter`
Pour qu'un composant enfant notifie son parent d'un événement ou lui envoie des données, nous utilisons `@Output()` en combinaison avec `EventEmitter`. L'enfant émet un événement, et le parent y réagit en écoutant cet événement via la liaison d'événements (event binding).

**Comment ça marche :**
1.  **Dans le composant enfant** :
    ```typescript
    import { Component, Output, EventEmitter } from '@angular/core';

    @Component({
      selector: 'app-enfant',
      template: '<button (click)="notifierParent()">Cliquez-moi</button>'
    })
    export class EnfantComponent {
      @Output() evenementClique = new EventEmitter<string>(); // Déclare un Output

      notifierParent() {
        this.evenementClique.emit('Je suis l\'enfant et j\'ai été cliqué !'); // Émet l'événement
      }
    }
    ```
2.  **Dans le composant parent** :
    ```html
    <app-enfant (evenementClique)="gererClicEnfant($event)"></app-enfant>
    ```
    ```typescript
    import { Component } from '@angular/core';
    // ...
    export class ParentComponent {
      gererClicEnfant(message: string) {
        console.log(message); // Affiche "Je suis l'enfant et j'ai été cliqué !"
      }
    }
    ```
    La variable `$event` contient les données émises par l'enfant.

### 3. Accès Direct au Composant Enfant : `@ViewChild()`
Parfois, un composant parent a besoin d'accéder directement à une instance de son composant enfant ou à un élément du DOM pour appeler une méthode ou manipuler une propriété. C'est le rôle de `@ViewChild()`.

**Comment ça marche :**
1.  **Dans le composant enfant** : Assurez-vous qu'il ait une méthode ou propriété publique à laquelle le parent peut accéder.
2.  **Dans le composant parent** :
    ```typescript
    import { Component, ViewChild, ElementRef, AfterViewInit } from '@angular/core';
    import { EnfantComponent } from './enfant.component'; // Importe le composant enfant

    @Component({
      selector: 'app-parent',
      template: `
        <app-enfant></app-enfant>
        <button (click)="appelerMethodeEnfant()">Appeler méthode enfant</button>
        <div #monDiv>Ceci est un div.</div>
      `
    })
    export class ParentComponent implements AfterViewInit {
      @ViewChild(EnfantComponent) enfantComponent!: EnfantComponent; // Accède à l'instance du composant
      @ViewChild('monDiv') monDivElement!: ElementRef; // Accède à un élément DOM par sa référence locale

      ngAfterViewInit() {
        // L'enfantComponent et monDivElement sont disponibles ici
        console.log(this.enfantComponent);
        this.monDivElement.nativeElement.style.backgroundColor = 'yellow';
      }

      appelerMethodeEnfant() {
        // Supposons que EnfantComponent ait une méthode 'direBonjour()'
        // this.enfantComponent.direBonjour();
      }
    }
    ```
    L'option `static: true` ou `static: false` dans `@ViewChild()` contrôle quand l'élément est résolu. `static: true` si l'élément est disponible avant la détection de changements, `static: false` si l'élément est à l'intérieur d'un `*ngIf` ou d'un `@for` et est résolu après.

Il est généralement recommandé d'utiliser `@ViewChild()` avec parcimonie, car un couplage trop fort entre parent et enfant peut rendre le code difficile à maintenir.

### 4. Communication entre Composants Non Liés : Les Services Partagés
Lorsque les composants n'ont pas de relation parent-enfant directe (par exemple, deux composants sur des routes différentes ou des composants frères), l'approche la plus flexible est d'utiliser un service partagé injecté dans les deux composants. Ce service peut utiliser des Observables RxJS (comme `Subject` ou `BehaviorSubject`) pour créer un canal de communication.

**Comment ça marche :**
1.  **Créez un service partagé** :
    ```typescript
    import { Injectable } from '@angular/core';
    import { Subject } from 'rxjs';

    @Injectable({
      providedIn: 'root' // Rendre le service disponible globalement
    })
    export class CommunicationService {
      private messageSource = new Subject<string>();
      message$ = this.messageSource.asObservable(); // Observable pour que les composants s'y abonnent

      envoyerMessage(message: string) {
        this.messageSource.next(message); // Émet le message
      }
    }
    ```
2.  **Dans les composants qui veulent communiquer** : Injectez le `CommunicationService`. L'un envoie des messages, l'autre s'abonne pour les recevoir.

    **Composant A (Envoi) :**
    ```typescript
    import { Component } from '@angular/core';
    import { CommunicationService } from './communication.service';

    @Component({ selector: 'app-comp-a', template: '<button (click)="envoyer()">Envoyer à B</button>' })
    export class CompAComponent {
      constructor(private commService: CommunicationService) {}
      envoyer() { this.commService.envoyerMessage('Bonjour de A !'); }
    }
    ```
    **Composant B (Réception) :**
    ```typescript
    import { Component, OnInit, OnDestroy } from '@angular/core';
    import { CommunicationService } from './communication.service';
    import { Subscription } from 'rxjs';

    @Component({ selector: 'app-comp-b', template: '<p>Reçu : {{ messageRecu }}</p>' })
    export class CompBComponent implements OnInit, OnDestroy {
      messageRecu: string = '';
      private subscription: Subscription = new Subscription();

      constructor(private commService: CommunicationService) {}

      ngOnInit() {
        this.subscription = this.commService.message$.subscribe(message => {
          this.messageRecu = message;
        });
      }

      ngOnDestroy() {
        this.subscription.unsubscribe(); // Éviter les fuites de mémoire
      }
    }
    ```

### 5. Gestion d'État Globale (Avancé)
Pour les applications de grande envergure avec un état complexe partagé par de nombreux composants, des solutions de gestion d'état comme NgRx (Redux pour Angular) ou Akita (plus simple) peuvent être envisagées. Elles fournissent un magasin d'état centralisé et des mécanismes structurés pour modifier et accéder à cet état, simplifiant la maintenance des applications complexes.

### Conclusion
La communication entre composants est un pilier de toute application Angular. En maîtrisant `@Input()` et `@Output()` pour les interactions parent-enfant, `@ViewChild()` pour l'accès direct ponctuel, et les services partagés avec RxJS pour les composants non liés, vous disposerez d'une boîte à outils complète pour concevoir des applications modulaires et réactives. Choisissez la méthode la plus appropriée en fonction de la relation entre vos composants et de la complexité de l'information à échanger.

### Sources
*   [Angular Input Output](https://blog.angular-university.io/angular-input-output/)
*   [Angular ViewChild](https://blog.angular-university.io/angular-viewchild/)
*   [Angular Component Communication](https://blog.angular-university.io/angular-component-communication/)

---

## L'Injection de Dépendances dans Angular : Un Guide Complet

### Introduction
L'Injection de Dépendances (DI) est un pilier fondamental de la conception d'applications Angular. C'est un principe de conception logicielle qui permet à une classe de recevoir ses dépendances depuis une source externe plutôt que de les créer elle-même. Dans Angular, la DI facilite la construction d'applications modulaires, testables et réutilisables. Comprendre ce mécanisme est crucial pour écrire du code propre et efficace.

### Qu'est-ce que l'Injection de Dépendances ?
Imaginez que vous avez un service `LoggerService` qui est responsable de la journalisation des messages. Sans DI, chaque composant ou service qui a besoin de journaliser créerait une nouvelle instance de `LoggerService` :

```typescript
// Sans DI
class MonComposant {
  logger = new LoggerService(); // Crée la dépendance directement
  // ...
}
```

Avec la DI, le `LoggerService` est fourni par le système d'injection d'Angular, et `MonComposant` déclare simplement qu'il en a besoin dans son constructeur :

```typescript
// Avec DI
class MonComposant {
  constructor(private logger: LoggerService) { // Reçoit la dépendance via DI
    // ...
  }
}
```

**Les avantages sont multiples :**
*   **Testabilité** : Vous pouvez facilement "simuler" (mock) des dépendances pour les tests unitaires.
*   **Maintenabilité** : Les classes sont moins couplées, ce qui facilite les changements.
*   **Réusabilité** : Les dépendances peuvent être partagées ou configurées différemment sans modifier la logique métier.
*   **Modularité** : Chaque composant est responsable de ses propres fonctionnalités, pas de la création de ses dépendances.

### Créer un Service Injectable
Pour qu'une classe puisse être injectée, elle doit généralement être marquée avec le décorateur `@Injectable()` :

```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root' // Le service est disponible globalement et optimisé pour le tree-shaking
})
export class LoggerService {
  log(message: string) {
    console.log(`LOG: ${message}`);
  }
}
```
L'option `providedIn: 'root'` est la méthode recommandée. Elle indique à Angular de créer une instance unique de ce service au niveau racine de l'application, et de la rendre disponible partout. C'est également optimisé pour le "tree-shaking", c'est-à-dire que si le service n'est jamais injecté nulle part, il ne sera pas inclus dans le bundle final de l'application.

D'autres options pour `providedIn` incluent `platform` (disponible pour toutes les applications Angular sur la page), `any` (instance unique par module paresseusement chargé), ou vous pouvez le déclarer dans le tableau `providers` d'un composant ou d'un module spécifique.

### L'Hiérarchie des Injecteurs
Angular organise les injecteurs en une hiérarchie, reflétant la structure de votre application.
*   **Injecteur racine** : Créé au démarrage de l'application. Tous les services avec `providedIn: 'root'` ou configurés dans `bootstrapApplication` y résident.
*   **Injecteurs de module** : Si vous utilisez encore des NgModules, chaque module lazy-loaded (chargé paresseusement) obtient son propre injecteur, qui est un enfant de l'injecteur racine.
*   **Injecteurs de composant** : Chaque composant obtient son propre injecteur par défaut. Si vous définissez des providers dans le tableau `providers` d'un `@Component`, une nouvelle instance de ce service sera créée pour ce composant et ses enfants.

Lorsqu'un composant demande une dépendance, Angular la recherche d'abord dans son propre injecteur, puis remonte la hiérarchie jusqu'à l'injecteur racine. Si une dépendance n'est pas trouvée, une erreur est générée.

### Contrôler l'Injection avec des Décorateurs
Angular offre des décorateurs pour affiner le comportement de l'injection :
*   `@Host()` : Recherche la dépendance uniquement sur l'injecteur du composant hôte et ses ancêtres, mais pas au-delà du composant hôte (utile pour les directives sur un composant par exemple).
*   `@Self()` : Recherche la dépendance uniquement sur l'injecteur du composant actuel.
*   `@SkipSelf()` : Recherche la dépendance à partir de l'injecteur parent (en ignorant l'injecteur du composant actuel).
*   `@Optional()` : Si la dépendance n'est pas trouvée, la valeur `null` est injectée au lieu de lever une erreur.

### La Fonction `inject()` : La Nouvelle Approche
Avec l'avènement des composants Standalone, la fonction `inject()` est devenue la méthode moderne et recommandée pour injecter des dépendances. Elle peut être utilisée dans les constructeurs, mais aussi dans les initialisateurs de champ, les usines (factories) ou les fonctions, rendant l'injection plus flexible.

```typescript
import { Component, inject } from '@angular/core';
import { LoggerService } from './logger.service';

@Component({
  selector: 'app-mon-composant',
  standalone: true,
  template: '<button (click)="logMessage()">Log</button>'
})
export class MonComposant {
  // Injection via le constructeur (toujours valide)
  // constructor(private logger: LoggerService) {}

  // Nouvelle méthode avec inject()
  private logger = inject(LoggerService);

  logMessage() {
    this.logger.log('Message via inject() !');
  }
}
```
`inject()` offre plus de flexibilité car il n'est pas limité aux constructeurs et peut être appelé dans n'importe quel contexte d'injection, à condition d'être appelé pendant la phase de construction du contexte (avant qu'Angular ne termine l'initialisation).

### Providers Personnalisés
Angular permet de remplacer une dépendance par une implémentation différente ou une valeur spécifique. Ceci est très utile pour les tests ou pour des configurations complexes. Vous utilisez des objets de configuration de provider :
*   `useClass` : Fournit une classe différente à la place de la classe demandée.
*   `useValue` : Fournit une valeur statique (un objet, une chaîne, etc.).
*   `useFactory` : Fournit une fonction d'usine qui crée la dépendance.
*   `useExisting` : Fournit un alias pour un service déjà enregistré.

```typescript
// Exemple de provider personnalisé
providers: [
  { provide: LoggerService, useClass: DevelopmentLoggerService } // Utilise un Logger de dev
]
```

### Conclusion
L'Injection de Dépendances est un concept puissant qui sous-tend la flexibilité et la testabilité d'Angular. En comprenant le rôle des `@Injectable()`, l'hiérarchie des injecteurs, les décorateurs de contrôle, et l'utilisation moderne de la fonction `inject()`, vous serez en mesure de concevoir des applications plus modulaires, plus faciles à maintenir et à tester. C'est une compétence essentielle pour tout développeur Angular.

### Sources
*   [Angular Dependency Injection Tutorial](https://blog.angular-university.io/angular-dependency-injection-tutorial/)

---

## Les Pipes Angular : Transformer les Données pour l'Affichage

### Introduction
Dans le développement web, il est courant de vouloir afficher des données sous un format différent de leur valeur brute. Par exemple, une date `Date()` est rarement présentée telle quelle à l'utilisateur, on préférera un format `JJ/MM/AAAA`. C'est exactement le rôle des Pipes Angular : ce sont des fonctions pures qui transforment les données directement dans les templates, de manière déclarative et réutilisable, sans modifier la logique de votre composant.

### Le Principe des Pipes
Un Pipe prend une valeur en entrée et retourne une nouvelle valeur transformée. Ils sont appliqués directement dans le template Angular en utilisant le caractère `|` (pipe).

```html
<p>Date actuelle : {{ dateActuelle | date:'shortDate' }}</p>
<p>Montant : {{ montant | currency:'EUR':'symbol':'1.2-2' }}</p>
```

### Les Pipes Intégrés d'Angular
Angular fournit une riche collection de Pipes prêts à l'emploi pour les transformations courantes :

*   **`DatePipe`** : Formate les dates selon diverses options (`'short'`, `'medium'`, `'long'`, ou un format personnalisé comme `'dd/MM/yyyy'`).
*   **`CurrencyPipe`** : Formate les nombres en devises, avec des symboles et des codes (ex: `1234.56 | currency:'EUR'`).
*   **`DecimalPipe`** : Formate les nombres décimaux, contrôlant le nombre de chiffres avant et après la virgule (ex: `3.14159 | number:'1.2-2'`).
*   **`JsonPipe`** : Convertit un objet JavaScript en chaîne JSON, utile pour le débogage (`{{ monObjet | json }}`).
*   **`UpperCasePipe` / `LowerCasePipe`** : Convertit une chaîne en majuscules ou minuscules.
*   **`SlicePipe`** : Extrait une partie d'une chaîne ou d'un tableau (ex: `['a', 'b', 'c'] | slice:1:2`).
*   **`AsyncPipe`** : Gère l'abonnement et le désabonnement aux Observables ou Promises, et affiche leur dernière valeur émise. C'est l'un des plus puissants car il automatise la gestion des flux asynchrones dans le template.

### Chaîner les Pipes et Utiliser des Paramètres
Vous pouvez combiner plusieurs Pipes en les chaînant. L'output d'un Pipe devient l'input du Pipe suivant. Vous pouvez également passer des paramètres aux Pipes pour personnaliser leur comportement.

```html
<p>Nom formaté : {{ 'jean dupont' | uppercase | slice:0:4 }}</p>
<!-- Output: "JEAN" -->
```

### Pipes Purs vs. Impurs : Performance et Détection de Changements
C'est un concept crucial pour comprendre la performance des Pipes :
*   **Pipes Purs (par défaut)** : Un Pipe pur n'est ré-exécuté que si son *input primitif change* (comme une chaîne, un nombre, un booléen) ou si la *référence de l'objet input change*. Si vous modifiez une propriété d'un objet passé à un Pipe pur sans changer la référence de l'objet lui-même, le Pipe ne sera pas ré-exécuté. C'est l'option la plus performante.
*   **Pipes Impurs** : Un Pipe impur est exécuté à *chaque cycle de détection de changements* d'Angular. Ils peuvent être utiles pour des transformations qui dépendent d'un état interne ou d'un appel asynchrone, mais ils doivent être utilisés avec parcimonie en raison de l'impact potentiel sur les performances. L'`AsyncPipe` est un exemple de Pipe impur intégré, mais il gère très bien ses performances internes. Pour créer un Pipe impur personnalisé, il faut définir `pure: false` dans le décorateur `@Pipe`.

### Créer vos Propres Pipes Personnalisés
Lorsque les Pipes intégrés ne suffisent pas, vous pouvez créer vos propres Pipes pour des logiques de transformation spécifiques à votre application.

1.  **Générez un Pipe** :
    ```bash
    ng generate pipe mon-custom
    ```
2.  **Implémentez l'interface `PipeTransform`** :
    Votre classe de Pipe doit être décorée avec `@Pipe()` et implémenter la méthode `transform()`.

    ```typescript
    import { Pipe, PipeTransform } from '@angular/core';

    @Pipe({
      name: 'monCustom', // Nom utilisé dans le template
      standalone: true // Si vous utilisez les composants Standalone
    })
    export class MonCustomPipe implements PipeTransform {
      transform(value: string, prefixe?: string): string {
        // La logique de transformation. 'value' est l'input principal.
        // 'prefixe' est un paramètre optionnel.
        return prefixe ? `${prefixe} ${value}` : value;
      }
    }
    ```
3.  **Utilisez-le dans votre template** :
    ```html
    <p>{{ 'mon texte' | monCustom:'Prefixe:' }}</p>
    <!-- Output: "Prefixe: mon texte" -->
    ```
    Si vous n'utilisez pas de composants Standalone, n'oubliez pas de déclarer votre Pipe dans un `NgModule` (typiquement `AppModule` ou un module de fonctionnalités) pour qu'il soit utilisable.

### Conclusion
Les Pipes Angular sont un outil puissant et élégant pour formater et transformer les données directement dans vos templates. Ils favorisent la séparation des préoccupations en déplaçant la logique de présentation hors des composants. En comprenant les Pipes intégrés, la distinction entre Pipes purs et impurs, et en sachant créer vos propres Pipes personnalisés, vous pouvez rendre vos templates plus lisibles, plus concis et plus efficaces, améliorant ainsi l'expérience utilisateur et la maintenabilité de votre code.

### Sources
*   [Angular Pipes](https://blog.angular-university.