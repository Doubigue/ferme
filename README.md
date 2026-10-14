# AgriSmart Farm Manager

Application web PWA en français : vitrine publique (`index.html`) et administration (`admin.html`). Interface responsive inspirée de Flutter, HTML/Tailwind CDN/JavaScript ES modules/Firebase Firestore.

## Démarrer

1. Décompressez le ZIP.
2. Ouvrez le dossier dans VS Code ou un éditeur.
3. Lancez un serveur web local (recommandé : extension Live Server) ou publiez le dossier sur un hébergeur HTTPS comme Netlify ou Firebase Hosting. **N'ouvrez pas les fichiers en `file://`** : les modules ES, Firebase et le service worker exigent un serveur.
4. `index.html` est le site public. Le bouton « Admin » ouvre `admin.html`.
5. La configuration Firebase est séparée dans `firebase.config.js`.

## Offline-first et données

- Un miroir local dans `localStorage` rend les enregistrements immédiatement disponibles hors connexion.
- Les écritures sont placées dans une file locale et tentées de nouveau au retour du réseau.
- Firestore utilise la persistance IndexedDB quand le navigateur la prend en charge.
- Le service worker met en cache les fichiers de l'application ; une première ouverture en ligne est nécessaire pour installer le shell et charger les CDN.
- Exportez régulièrement une sauvegarde JSON depuis « Sauvegarder les données ».

## Collections Firestore

`animals`, `transactions`, `cares`, `stocks`. Chaque document est identifié par un ID généré côté navigateur. Les rappels sont contrôlés toutes les 15 secondes lorsque la page tourne ; le navigateur peut suspendre les minuteries en arrière-plan. Le son doit être activé par un geste utilisateur.

## Sécurité Firebase — à lire avant publication

Comme demandé, l'espace admin est accessible depuis le site public sans écran de connexion. **Un lien vers `admin.html` n'est pas une protection d'accès.** Les règles Firestore restent indispensables : sans règles sûres, n'importe quel visiteur pourrait lire, modifier ou supprimer les données de la ferme. Ne déployez pas de règles `allow read, write: if true` en production. Pour une utilisation réelle, activez Firebase Authentication et restreignez les écritures à un compte administrateur autorisé.

## Limites actuelles

- L'export PDF utilise la boîte de dialogue d'impression du navigateur : choisir « Enregistrer au format PDF » pour garder les couleurs. Cela évite une dépendance PDF externe fragile.
- Les alertes sonores ne peuvent pas être garanties lorsque le navigateur est fermé ou que le système suspend la page. Pour des notifications garanties en arrière-plan, il faut une architecture de notifications push et un serveur ou Cloud Functions.
- Les photos sont chargées depuis Unsplash via Internet ; les images ne sont pas embarquées dans le ZIP.
- Les données de ce starter sont opérationnelles mais ne constituent pas un système de comptabilité certifié ni un dossier vétérinaire.
