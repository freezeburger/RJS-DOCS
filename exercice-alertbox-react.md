
# 🎯 Exercice – Créer un composant `AlertBox` avec backdrop et fermeture au clic extérieur

## 📝 Contexte

Vous concevez un système de notifications utilisateur. Les alertes doivent être **visuellement distinctes**, **accessibles**, et **facilement fermables**.

---

## 🧩 Objectifs pédagogiques

- Créer un composant graphique réutilisable (`AlertBox`) avec **props dynamiques**.
- Afficher un **backdrop semi-transparent** sous le composant.
- Implémenter une **fermeture par clic global** en dehors du composant (`useRef` + `useEffect`).
- Gérer un **état local de visibilité** (ou contrôlé via prop).

---

## ✅ Instructions

1. Créez un composant `AlertBox` avec les propriétés suivantes :
   - `type` : `"info" | "warning" | "error" | "success"`
   - `message` : texte court affiché dans l'alerte.
   - `closable` *(optionnel, default: `true`)* : permet d’afficher un bouton de fermeture.
   - `onClose` : fonction appelée lors de la fermeture (par bouton ou clic global).
   - `children` *(optionnel)* : contenu JSX alternatif au message.

2. Affichez un **backdrop** :
   - Un `div` couvrant l’écran en `position: fixed`, avec un fond noir semi-transparent.
   - Le `AlertBox` est centré au-dessus.

3. Implémentez la **fermeture au clic en dehors de la boîte** :
   - Utiliser `useRef` pour détecter si le clic est en dehors.
   - Fermer automatiquement la boîte via `onClose()`.

4. Bonus :
   - Ajouter une animation d’apparition/disparition (opacity + scale).
   - Empêcher le scroll de la page quand l’alerte est visible.

---

## 💡 Exemple d’usage

```tsx
<AlertBox
  type="warning"
  message="Attention : vos modifications ne sont pas enregistrées."
  onClose={() => setShow(false)}
/>

<AlertBox type="success" onClose={() => console.log("fermé")}>
  ✅ Opération réussie ! <a href="/details">Voir les détails</a>
</AlertBox>
```
