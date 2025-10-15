
# 🧩 Exercice — Création de 3 Containers autour d’un Service de Notifications

Dans cet exercice, vous allez structurer une fonctionnalité de notifications en 3 **containers réactifs** (composants à logique intégrée), en **vue de préparer l’introduction de Redux** dans le projet.

## 🎯 Objectif

Construire trois composants conteneurs cohérents autour d’un **service CRUD local en mémoire**, chacun ayant un rôle bien défini :

---

## 📦 Spécifications du modèle

Vous utiliserez les interfaces suivantes pour structurer les données manipulées par les composants :

```ts
export interface UILevel {
  level: 'primary' | 'optional' | 'critical';
}

export interface NotificationModel extends UILevel, WithUniqueId {
  message: string;
  time: number;
}

export interface WithUniqueId {
  id: UniqueId;
}

export interface CrudService<T extends WithUniqueId> {
  data: Array<T>;
  create(item: Omit<T, 'id'>): Promise<T>;
  read(): Promise< T[]>;
  read(id: T['id']): Promise<T | null>;
  read(id?: T['id']): Promise<T | T[] | null>;
  update(item: T, update: Partial<Omit<T, 'id'>>): Promise<T>;
  delete(id: T['id']): Promise<T>;
}
```

> Exemple de dataset :

```json
[
  {
    "id": "1",
    "level": "primary",
    "message": "Utilise `useEffect` pour synchroniser avec des effets externes, mais évite d’y mettre de la logique métier.",
    "time": 1694952000000
  },
  {
    "id": "2",
    "level": "optional",
    "message": "Préfère des composants fonctionnels avec hooks plutôt que des classes.",
    "time": 1694955600000
  },
  {
    "id": "3",
    "level": "critical",
    "message": "Ne modifie jamais directement les props ou le state : utilise `setState` ou les setters des hooks.",
    "time": 1694959200000
  },
  {
    "id": "4",
    "level": "primary",
    "message": "Sépare la logique métier des composants visuels à l’aide de hooks personnalisés.",
    "time": 1694962800000
  },
  {
    "id": "5",
    "level": "optional",
    "message": "Utilise `React.memo` pour éviter les rerenders inutiles de composants.",
    "time": 1694966400000
  },
  {
    "id": "6",
    "level": "critical",
    "message": "N’oublie pas de nettoyer les effets (`useEffect`) avec une fonction de retour pour éviter les fuites mémoire.",
    "time": 1694970000000
  },
  {
    "id": "7",
    "level": "primary",
    "message": "Gère les erreurs avec des Error Boundaries ou un composant de fallback.",
    "time": 1694973600000
  },
  {
    "id": "8",
    "level": "optional",
    "message": "Structure ton code en composants réutilisables et cohérents.",
    "time": 1694977200000
  },
  {
    "id": "9",
    "level": "critical",
    "message": "Évite de stocker des données dérivées dans le state : dérive-les dynamiquement à partir des props.",
    "time": 1694980800000
  },
  {
    "id": "10",
    "level": "primary",
    "message": "Typage TypeScript : typage explicite des props, retour de hook, et évite `any`.",
    "time": 1694984400000
  }
]
```

---

## 🧱 Conteneurs à créer

### 1. `NotificationBoard`

> Composant affichant la liste des notifications actuellement en mémoire.

- Affiche chaque notification avec son message, son niveau (`level`) et son horodatage.
- Ajoute un bouton de suppression pour chaque notification.
- Utilise un hook ou une logique embarquée (`useLogic`) pour interagir avec le service.

---

### 2. `NotificationAccessor`

> Composant rend la **dernière notification ajoutée**, mais uniquement pendant 3 secondes.

- Accepte une `render function` via `children`, permettant de customiser l’affichage :

```tsx
<NotificationAccessor>
  {notification => notification && <Banner>{notification.message}</Banner>}
</NotificationAccessor>
```

- Doit gérer le délai d'affichage automatique de 3 secondes à l’aide d’un `setTimeout`.
- Reste réactif à l’ajout d’une nouvelle notification.

---

### 3. `NotificationEditor`

> Composant formulaire pour créer une nouvelle notification.

- Utilise `react-hook-form` ou un autre système de formulaire simple.
- Champs :
  - `message` (texte, requis)
  - `level` (select parmi `'primary'`, `'optional'`, `'critical'`)
- À la soumission, appelle `notificationService.create(...)` et réinitialise le formulaire.

---

## 🛠 Service attendu

Créer un fichier `notification.service.ts` exposant une implémentation de l’interface `CrudService<NotificationModel>` avec :

- Une liste interne simulant un backend (par exemple : un tableau en mémoire).
- Une génération automatique d’identifiants.
- Des méthodes `create`, `read`, `update`, `delete` conformes à l’interface.

---

## 🔁 Contraintes

- Le service ne doit **pas** utiliser de `useState` — il est **stateless** côté React.
- Le service doit implémenter `CrudService`.
- Vous êtes libre d’utiliser un système de `subscribe/notify` artisanal ou une abstraction type `mitt`.

---

## 🚀 Objectif pédagogique

- Apprendre à structurer des composants à logique forte (containers).
- Maîtriser les points d’entrée (formulaire), les points de sortie (vue), et la gestion intermédiaire (accès au service).
- Préparer mentalement la transition vers un **store Redux**, qui centralisera cette logique et la rendra observable globalement.

---

**🧪 Bonus** : Ajoutez un petit `test.test.tsx` pour vérifier :
- L’affichage de la notification créée.
- Le rendu temporaire de `NotificationAccessor`.
- La suppression d’une notification.

---

💡 *Pensez à typer vos props, à isoler la logique dans un hook, et à soigner la clarté de chaque composant. Cette base vous servira pour l'intégration prochaine dans Redux.*

# ✅ Grille de Critères de Test — Exercice Notifications (Containers + Service)

Cette grille liste les tests attendus pour valider les comportements des 3 containers (`NotificationBoard`, `NotificationEditor`, `NotificationAccessor`) ainsi que du service `notification.service.ts`.

---

## 🔧 1. notification.service.ts

**Tests unitaires :**

- [ ] `create` ajoute un élément avec un `id` généré automatiquement.
- [ ] `read()` sans argument retourne tous les éléments.
- [ ] `read(id)` retourne l’élément correspondant.
- [ ] `update(item, update)` met à jour les champs sans modifier l’`id`.
- [ ] `delete(id)` supprime l’élément concerné.
- [ ] Toutes les méthodes retournent une `Promise`.
- [ ] (Si bus d’événement) `create` et `delete` émettent un événement.

---

## 📝 2. NotificationEditor

**Tests comportementaux :**

- [ ] Le formulaire affiche les champs `message` (texte) et `level` (select).
- [ ] Le champ `message` est requis — un message d’erreur s’affiche si vide.
- [ ] La soumission du formulaire appelle `notificationService.create(...)`.
- [ ] Le formulaire est réinitialisé après soumission.
- [ ] Le `console.log(data)` ou l’appel au service peut être espionné avec `vi.spyOn(...)`.
- [ ] (Bonus) Un retour visuel s'affiche (toast, label, etc.).

---

## 📋 3. NotificationBoard

**Tests comportementaux avec données simulées :**

- [ ] Affiche la liste des notifications en fonction des données reçues.
- [ ] Chaque notification affiche : `message`, `level`, `time`.
- [ ] Chaque notification comporte un bouton "Supprimer".
- [ ] Le clic sur "Supprimer" appelle `notificationService.delete(id)`.
- [ ] La liste se met à jour après suppression.
- [ ] (Bonus) Un message s’affiche si aucune notification n’est présente.

---

## ⚡ 4. NotificationAccessor

**Tests fonctionnels liés au temps :**

- [ ] Lorsqu’une notification est émise, elle est affichée via le `children` render.
- [ ] La notification est visible pendant **3 secondes**, puis disparaît.
- [ ] Une nouvelle notification remplace la précédente si elle arrive dans les 3 secondes.
- [ ] Le composant appelle bien `children(notification)` même si `null`.

> 🧪 Utiliser `vi.useFakeTimers()` ou `jest.useFakeTimers()` pour tester les délais.

---

## 🔁 5. Tests croisés (intégration)

- [ ] Lorsqu’un élément est ajouté via `NotificationEditor`, il apparaît dans `NotificationBoard`.
- [ ] La suppression dans `NotificationBoard` modifie le comportement de `NotificationAccessor`.

---

## 🧑‍🏫 Compétences visées

- Maîtrise du **cycle de données React** (formulaire → service → affichage).
- Séparation des responsabilités : composants, logique, service.
- Capacités à **tester l'interaction UI/Service**.
- Préparation à Redux : logique déclenchée, état modifié, vue réactive.

---