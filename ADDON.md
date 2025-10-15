# Roadmap

 * State MPanagement
 * Form
 * Router


# Niveaux (du concret vers le générique)

1. Pragmatique (feature code)
Fait la tâche. Utilise des abstractions existantes pour rester lisible.

2. Abstraction légère (issue d’une refactorisation)
Extrait un petit morceau récurrent (fonction, composable, pipe…) ; dépend encore du domaine/app.

3. Utilitaire applicatif (abstraction générique interne)
Réutilisable à l’échelle de l’app/monorepo ; API plus stable ; tests + doc.

4. Utilitaire agnostique (lib/SDK)
Orienté “produit” : versionné, documenté, potentiellement open-sourcé ; abstraction plus profonde si nécessaire.

#  Abstraction
> Ce qu’est une “bonne abstraction”

- **Utile** : elle réduit la complexité perçue là où la complexité est vécue.

- **Connue/Partagée** : nommage clair + documentation courte + exemples ; l’équipe sait quand l’utiliser.

- **Mono-responsabilité** : un seul motif de changement.

(bonus prudents) Testable, remplaçable, stable (peu dépendante du contexte applicatif), économique (coût < bénéfice).

#  Angular

- NgModule  - Niveau 2 (Organisation)
- Component - Niveau 1 
- Directive - Niveau 2 / (2>3)
- Pipe      - Niveau 2 
- Service   - Niveau 3 / (3>2)

- Librairie - Niveau 4

#  Primitives Applicatives

> Objets de valeurs applicatifs
- Functionnel : User, Product...
- Technique : Message, NOtification, LogEntry