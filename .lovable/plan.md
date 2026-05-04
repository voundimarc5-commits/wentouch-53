## Objectif

1. Tester les animations et le sizing des éléments (formulaires, CTAs, cartes, sticky bar, footer, etc.) sur mobile (375px) et desktop (1366px+).
2. Corriger les problèmes détectés.
3. Créer deux subpages temporaires : **Mentions légales** et **Politique de confidentialité**, déjà liées dans le footer (`showSubpage('mentions-legales')` et `showSubpage('confidentialite')`) mais qui n'existent pas encore (warning console actuel : "Subpage not found").

## Étape 1 — QA Animations & Sizing

Le sandbox preview a renvoyé une erreur HTTP 412 lors du test live, je vais donc procéder par :
- Audit statique du CSS d'`index.html` (responsive, breakpoints, classes `rv`, formulaires `gv3-field`, sticky bar, WhatsApp FAB, hero, accordéon).
- Re-test live via navigateur après le passage en mode build (le sandbox redevient accessible).

Zones auditées sur les deux viewports :

| Zone | Élément | Vérifications |
|---|---|---|
| Hero | `#hero-logo-anim`, `.qlink-input`, CTAs, `.hero-cred-count` | Taille input full-width mobile, animation logo non coupée |
| Catégories | `.ccard` (Obsidian Gallery) | Aspect ratio mobile, reveal |
| Problème/Solution | `.ps-col`, blur blocks | Empilement vertical mobile |
| Process | `.proc-step`, `.qsn-inline` | Flow horizontal desktop / vertical mobile |
| Formulaire devis | `.gv3-field`, `.gv3-counter`, `.gv3-cta` | Largeur des inputs, padding mobile |
| Sticky bar | `.home-sticky` | Pas de chevauchement avec WhatsApp FAB |
| WhatsApp FAB | bouton flottant + label | Position bas-droite, espace safe-area iOS |
| Footer | `.flogo`, `.flinks`, `.fbottom` | Cascade reveal, alignement mobile |
| Subpages | `#sp-*` | Transitions fade in/out, scroll restauré |

## Étape 2 — Corrections probables

Sans encore le résultat live, les fixes types à appliquer si détectés :
- Forcer `width:100%` + `max-width:100%` sur `.qlink-input` et `.gv3-field input` en `<= 480px`.
- Ajouter `padding-bottom: 90px` sur `.home-sticky` ou décaler le FAB WhatsApp (`bottom: 80px` mobile) pour éviter le chevauchement avec la sticky bar.
- Vérifier que `.rv` ne reste pas invisible si l'observer ne se déclenche pas (fallback `setTimeout` 1.5s pour ajouter `.ok`).
- Safe-area iOS : `bottom: calc(20px + env(safe-area-inset-bottom))` sur le FAB.

## Étape 3 — Création des Mentions Légales (`sp-mentions-legales`)

Nouvelle subpage dans `index.html` (après les autres `<div id="sp-*">`), avec la même structure visuelle que les subpages existantes (`.subpage`, `.sp-section`, fond noir, accents or).

Contenu temporaire (FR) :
- **Éditeur du site** : Wentouch — Conciergerie d'achat Europe → Cameroun
- **Contact** : WhatsApp +237 692 052 155 — contact@wentouch.com
- **Hébergeur** : Lovable / Vercel
- **Propriété intellectuelle** : tous droits réservés, marques tierces propriété de leurs titulaires
- **Responsabilité** : Wentouch agit comme intermédiaire d'achat ; les marques affichées ne sont pas partenaires
- **Droit applicable** : droit camerounais et français
- Mention "Document temporaire — version définitive en cours de rédaction"

## Étape 4 — Création de la Politique de Confidentialité (`sp-confidentialite`)

Même structure visuelle. Sections :
- **Données collectées** : nom, WhatsApp, email, lien produit, adresse de livraison
- **Finalité** : traitement de la demande de devis et livraison
- **Canaux** : WhatsApp Business (+237 692 052 155), email
- **Conservation** : durée de la commande + 12 mois
- **Partage** : transporteurs et fournisseurs européens uniquement
- **Droits RGPD/Loi cam.** : accès, rectification, suppression — par WhatsApp ou email
- **Cookies** : aucun cookie de tracking, uniquement techniques
- Mention "Document temporaire — version définitive en cours de rédaction"

Chaque page :
- Titre éditorial (Playfair) en or
- Bouton retour vers home
- Animations `.rv` cohérentes
- Bandeau WhatsApp en bas pour rester dans le ton conciergerie

## Étape 5 — Re-test final

Après corrections, re-test navigateur en :
- 1366×768 (desktop)
- 390×844 (mobile)

Vérification : reveal, formulaire devis (3 étapes), navigation vers les nouvelles subpages légales, retour home avec scroll restauré.

## Détails techniques

- Tout reste dans `index.html` (règle Core memory : pas de React).
- Réutilisation des classes existantes : `.subpage`, `.sp-section`, `.sp-step-btn`, `.rv`, `.rv-d1..d4`, `.wa-band`.
- Aucune dépendance ajoutée.
- Les liens footer existent déjà → aucune modification du footer nécessaire, juste créer les `<div id="sp-mentions-legales">` et `<div id="sp-confidentialite">`.
