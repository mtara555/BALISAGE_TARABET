# Étiquettes Balisage — PWA Android (Citizen CMP-30IIBT / Xprinter XP-470B)

Application web installable (PWA), 100 % hors ligne après la première ouverture.
Étiquette **6,5 × 3,8 cm** : CODE, DESIGNATION, PRIX BARRÉ, PRIX DE VENTE (décimales en petit), code-barres Code128 et date.

## 1. Déploiement sur GitHub Pages
1. Créez un dépôt GitHub (ex. `balisage`) et envoyez **tous** les fichiers à la racine :
   `index.html`, `manifest.webmanifest`, `sw.js`, `icon-192.png`, `icon-512.png`, `exemple_etiquettes.xlsx`, `vendor/xlsx.full.min.js`, `README.md`.
2. GitHub → **Settings → Pages** → *Source* : `Deploy from a branch` → branche `main`, dossier `/ (root)` → **Save**.
3. Après 1 à 2 minutes, l'application est sur `https://<utilisateur>.github.io/balisage/`.
4. **Mise à jour** : après chaque modification, changez `VERSION` dans `sw.js` (ex. `balisage-v2`) pour forcer le rafraîchissement du cache.

## 2. Installation sur le téléphone Android
1. Ouvrez l'adresse dans **Chrome** → menu ⋮ → **Ajouter à l'écran d'accueil / Installer l'application**.
2. Installez aussi **RawBT** (Play Store, gratuit) : c'est lui qui envoie les données à l'imprimante en Bluetooth.

## 3. Configuration de l'imprimante Citizen CMP-30IIBT
1. Allumez l'imprimante → Paramètres Android → Bluetooth → **associez** la CMP-30IIBT (code PIN souvent `0000` ou `1234`).
2. Ouvrez **RawBT** → *Imprimante* → choisissez la CMP-30IIBT → mode **ESC/POS**, largeur **80 mm / 576 points**, résolution 203 dpi → *Test d'impression*.
3. Chargez le rouleau d'étiquettes. Si l'imprimante est réglée sur le capteur d'espace (gap) ou de repère noir (réglage Citizen, hors application), mettez **« Envoyer FF »** sur *Oui* dans les réglages de l'application pour qu'elle avance à l'étiquette suivante. Sinon (rouleau continu), utilisez **Espacement inter-étiquettes** (3 mm par défaut).


## 3 bis. Xprinter XP-470B (imprimante d'étiquettes, langage TSPL)
1. Dans l'application : ⚙️ Réglages → **Imprimante : Xprinter XP-470B**.
2. Choisissez la **connexion** :
   - **USB direct** : câble USB-OTG entre le téléphone et l'imprimante, Chrome sur Android. Au premier envoi, Android demande l'autorisation USB : acceptez.
   - **RawBT** : dans RawBT, choisissez l'imprimante en USB ou Bluetooth (selon votre exemplaire), puis utilisez le mode « RawBT ».
3. Chargez le rouleau 65 × 38 mm. Faites une **auto-calibration** du capteur d'espace (maintenir le bouton FEED à l'allumage, selon le manuel XP-470B).
4. Touchez **Test 1 étiquette**, puis ajustez si besoin :

| Symptôme | Réglage |
|---|---|
| Image en négatif (fond noir) | *Noir = bit 0 (TSPL)* → **Non** |
| Étiquette à l'envers | *Orientation* → **Rotation 180°** |
| Décalage entre étiquettes | *Espacement inter-étiquettes* = valeur de l'espace du rouleau (mm) |
| Rien ne sort | Imprimez la page d'auto-test et vérifiez que le langage est **TSPL** |

Les copies identiques d'une même ligne sont envoyées avec une seule image (`PRINT 1,n`), donc l'envoi est plus rapide.

## 4. Utilisation
1. **Importer Excel** : fichier .xlsx / .xls / .csv, ligne 1 = `CODE | DESIGNATION | PRIX BARREE | PRIX DE VENTE` (voir `exemple_etiquettes.xlsx`).
2. Corrigez les lignes dans le tableau, réglez **Qté**, supprimez avec ✖. Touchez une ligne pour voir son aperçu.
3. **Aperçu avant impression** : toutes les étiquettes.
4. **Test 1 étiquette** puis **Imprimer** : les étiquettes partent par lots (10 par défaut) ; touchez « Envoyer le lot suivant ».
5. **Exporter en PDF** : ouvre l'impression Android → *Enregistrer au format PDF* (secours si l'imprimante est indisponible).


## 4 bis. Travail 100 % mobile : scanner les articles au rayon
1. **📚 Charger le catalogue** : choisissez l'Excel complet (CODE | DESIGNATION | PRIX BARREE | PRIX DE VENTE). Il reste en mémoire du téléphone, à ne charger qu'à chaque nouvelle version du fichier.
2. **📷 Scanner** : la caméra s'ouvre. Visez le code-barres d'un article : il est ajouté à la liste « Articles à imprimer » (vibration + message). Scanner deux fois le même article augmente sa Qté.
3. Pas de caméra ou code abîmé : saisissez le code dans le champ et touchez **Ajouter** (un lecteur code-barres Bluetooth/USB qui « tape » le code fonctionne aussi).
4. Fermez la caméra, vérifiez la liste, puis **Imprimer**. **🗑️ Vider la liste** prépare la série suivante.
- Le code scanné est comparé à la colonne CODE (zéros de tête ignorés ; un code-barres qui contient le code article est aussi reconnu).
- Le scan caméra utilise Chrome sur Android à jour (fonction BarcodeDetector).

## 5. Réglages utiles
| Réglage | Rôle |
|---|---|
| Décalage gauche | Étiquette 65 mm sur zone imprimable 72 mm : 3,5 mm = centrée. À ajuster au test. |
| Largeur papier imprimable | 72 mm pour la CMP-30II (576 points à 203 dpi). |
| Espacement inter-étiquettes | Avance du papier après chaque étiquette. |
| Étiquettes par lot | Réduire (ex. 5) si RawBT refuse un lot trop gros. |

## 6. Règles de mise en page
- Désignation en majuscules : 3 lignes max, police réduite automatiquement, puis troncature avec « … ».
- Prix barré affiché seulement s'il est renseigné. Pas de « DH » ni de pourcentage.
- Prix non numérique : la ligne passe en rouge et l'impression est bloquée.
