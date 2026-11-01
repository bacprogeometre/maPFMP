# Ma PFMP

Application web où les élèves de Bac Pro Géomètre déposent, pendant leur PFMP, leurs photos et leurs fiches activités rattachées aux compétences C01 à C14. À la fin du stage, elle prépare la trame Word de leur rapport. Le professeur suit la classe, saisit les périodes PFMP 1 à 5 et commente les fiches.

Lycée Pierre Mendès France, Rennes. Version d'octobre 2026.

## Contenu du dossier

| Fichier | Rôle |
| --- | --- |
| `index.html` | L'application (élève et professeur, téléphone et ordinateur) |
| `supabase-pfmp.sql` | Création de la base de données, des règles d'accès et du stockage des photos |
| `manifest.webmanifest`, `sw.js`, `icons/` | Rendent l'application installable sur l'écran d'accueil des téléphones (nom « Ma PFMP », icône) |
| `README.md` | Ce guide |

Sans configuration, `index.html` s'ouvre en **mode démonstration** : les données restent dans le navigateur.

## Mise en place pour la classe (environ 15 minutes)

### 1. Créer la base de données (Supabase)

1. Créez un compte sur supabase.com, puis un projet (région européenne, par exemple Francfort).
2. Ouvrez **SQL Editor > New query**, collez tout le contenu de `supabase-pfmp.sql` et cliquez sur **Run**.
   Le script peut être relancé sans risque : il met à jour une installation existante sans effacer de données.
3. Dans **Authentication > Providers > Email**, désactivez **Confirm email**.
4. Dans **Project Settings > API**, notez l'**URL du projet** et la clé **anon public**.

### 2. Créer votre compte professeur

1. Dans **Authentication > Users > Add user**, créez l'utilisateur `prof.geometre@pfmp.pmf-rennes.fr` (ou un autre identifiant suivi de `@` et du domaine `EMAIL_DOMAIN`), avec un mot de passe, en cochant **Auto Confirm User**.
2. Dans **SQL Editor**, exécutez :

   ```sql
   update public.pfmp_profiles set role = 'prof', nom = 'VOTRE NOM', prenom = 'Prénom'
     where identifiant = 'prof.geometre';
   ```

Vous vous connecterez ensuite avec l'identifiant `prof.geometre` et ce mot de passe.

### 3. Configurer l'application

Ouvrez `index.html` dans un éditeur de texte et remplissez le bloc `CONFIGURATION` au début du script :

```js
const CONFIG = {
  SUPABASE_URL: "https://xxxx.supabase.co",
  SUPABASE_ANON_KEY: "votre clé anon public",
  EMAIL_DOMAIN: "pfmp.pmf-rennes.fr",
  BUCKET: "pfmp",
  LYCEE: "Lycée Pierre Mendès France — Rennes",
  LYCEE_COURT: "Lycée PMF Rennes",
  SIGLE: "PMF",
  DIPLOME: "Bac Pro Géomètre"
};
```

Un autre lycée n'a qu'à changer ces lignes (et utiliser son propre projet Supabase).

### 4. Mettre en ligne (GitHub Pages)

1. Déposez **tout le dossier** (y compris `icons/`, `manifest.webmanifest` et `sw.js`) dans un dépôt GitHub.
2. Dans **Settings > Pages**, choisissez la branche principale et le dossier racine.
3. L'adresse obtenue (en `https://`) est celle à donner aux élèves.

### 5. Démarrer avec la classe

1. Connectez-vous en professeur, ouvrez **Comptes élèves** et collez la liste « NOM Prénom » copiée depuis Pronote. Notez ou téléchargez les codes affichés : ils ne seront plus visibles ensuite.
2. Ouvrez **Périodes PFMP**, choisissez la période (1 à 5), appliquez les dates à la classe, puis saisissez entreprise, commune et tuteur pour chaque élève.
3. Donnez à chaque élève son identifiant, son code et l'adresse du site.

## Motiver les élèves

- **Jauge « Mon rapport est prêt à X % »** sur l'accueil de l'élève : présentation de l'entreprise, nombre de fiches, activités des pôles 1 et 2, compétences C01 à C11, analyse des fiches, photos légendées. Elle affiche les trois priorités du moment. Le pôle 3 (détection de réseaux) n'est pas compté : c'est un bonus.
- **Cheminement des compétences** : C01 à C11 forment une polygonale qui se ferme quand toutes sont rencontrées ; C12 à C14 forment une antenne bonus.
- **Badges** : Première fiche, Premier levé, Implanteur, Géoréférenceur, Œil du foncier, As du traitement, Reporter de terrain, Régulier, Belle plume, Cheminement fermé, Rapport prêt, et le bonus Détective des réseaux.
- **Retours en un geste** : sur chaque fiche, le professeur et le tuteur peuvent réagir (Bravo, Bien vu, À développer) ou écrire un mot. L'élève voit une pastille rouge sur « Mon dossier » tant qu'il n'a pas lu ses nouveaux retours.

## Lien tuteur (sans compte)

Chaque période de PFMP a un lien secret. Le professeur le trouve sur la fiche de l'élève (Périodes de PFMP > Lien du tuteur), l'élève dans l'onglet Mon stage. Envoyé au tuteur par SMS ou mail, il lui montre les fiches de cette PFMP (sans les photos) et lui permet de réagir. Le lien fonctionne jusqu'à 60 jours après la fin de la PFMP. « Nouveau lien » désactive l'ancien.

## Installer l'application sur un téléphone

- **Android (Chrome) :** ouvrir l'adresse, menu ⋮ > **Installer l'application** (ou **Ajouter à l'écran d'accueil**).
- **iPhone (Safari) :** ouvrir l'adresse, bouton Partager > **Sur l'écran d'accueil**.

L'icône « Ma PFMP » apparaît comme une application. Une connexion internet reste nécessaire pour envoyer les fiches et les photos.

## Mettre à jour

Remplacez `index.html` sur GitHub par la nouvelle version, en recopiant d'abord votre bloc `CONFIGURATION`. Si une nouvelle version de `supabase-pfmp.sql` est fournie, relancez-la dans SQL Editor (elle complète la base sans effacer de données).

## Données personnelles (élèves mineurs)

- Validez l'usage avec le chef d'établissement (responsable de traitement) et le DPO académique avant le lancement.
- Pas de visage sans accord, pas de document client confidentiel sans l'autorisation du tuteur : l'application le rappelle à chaque dépôt.
- Pour supprimer des élèves ou une classe entière, ouvrez **Comptes et classes** dans l'espace professeur : suppression d'un élève, d'une sélection d'élèves, ou d'une classe (il faut retaper le nom de la classe pour confirmer). Les comptes de connexion, fiches, photos, périodes et commentaires sont effacés ; rien n'est à faire dans Supabase. Votre compte professeur ne peut pas être supprimé de cette façon.
- Si vous aviez installé une version précédente, relancez `supabase-pfmp.sql` pour activer la suppression des comptes.
