# Baromètre étudiant 2026 — page « recevoir mon lien »

Page d'atterrissage du **QR code** présenté par les ambassadeurs sur les campus. Elle explique en trois étapes comment récupérer son lien d'enquête personnel, et propose trois façons d'envoyer le mail déclencheur (Gmail, application mail, copie de l'adresse).

Dispositif complet : livrable **L9** (`~/enquete_barometre/livrables/L9_qr_code_lien_personnel.md`) · mission MANEO **M-2026050**.

## Ce que fait la page

- **« Ouvrir mon application mail »** → `mailto:` — **unique bouton d'action**. Il ouvre l'application mail déjà configurée sur le téléphone, sans passer par le navigateur ni par un écran de connexion.
- *Écarté le 25/08 après test terrain : un bouton « Ouvrir dans Gmail »* (`https://mail.google.com/a/sciencespo.fr/mail/?view=cm&fs=1&tf=1&to=…&su=…`). Sur le papier il avait un avantage — le segment `/a/sciencespo.fr/` force le compte du domaine quand plusieurs comptes Google sont connectés. En pratique, le `mailto:` s'est révélé nettement plus fonctionnel sur les téléphones testés. Conséquence à assumer : **le choix du compte expéditeur repose désormais entièrement sur l'étudiant**, d'où l'avertissement renforcé sur la page (« vérifiez l'expéditeur avant d'envoyer »). À re-tester si le taux de rejets `rejected_external_domain` dans les logs s'avère élevé pendant la campagne.
- **Bouton « Copier »** → presse-papiers, avec repli `execCommand` pour les contextes non sécurisés.
- Bascule **FR / EN**, anglais par défaut si le navigateur n'est pas francophone.
- **Aucun cookie, aucun script tiers, aucune requête sortante.** Un seul fichier, 12 Ko.

Dans le `mailto:`, le `+` de l'adresse est laissé littéral : il n'est pas dans la partie requête, l'encoder casserait l'adresse.

## Déploiement (GitHub Pages)

```bash
cd ~/barometre-qr
git init && git add -A && git commit -m "Page QR Baromètre 2026"
gh repo create <compte>/barometre-qr --public --source=. --push
# puis : Settings → Pages → Source: main / (root)
```
URL obtenue : `https://<compte>.github.io/barometre-qr/`

## À tester avant impression du QR

- [x] Test terrain 25/08 : le bouton `mailto:` est le plus fonctionnel → seul bouton conservé
- [x] iOS (Apple Mail) et iOS (Gmail par défaut) → le composeur s'ouvre bien prérempli
- [x] Android → idem
- [x] Étudiant avec **plusieurs comptes** → observer quel compte l'app propose par défaut (risque connu, couvert par l'avertissement)
- [x] Bouton Copier sur mobile (Safari et Chrome)
- [x] Lisibilité en plein soleil, à bout de bras

## Points ouverts

1. **Hébergement — arbitré le 25/08 : on reste sur GitHub Pages**, qui s'est révélé parfaitement fonctionnel (HTTPS, disponibilité, mise à jour en trente secondes, historique versionné). Deux conditions, dans cet ordre d'importance :
   - **Propriété du dépôt.** Il vit aujourd'hui sous un compte individuel : un QR imprimé sur des centaines de supports ne peut pas dépendre d'un compte personnel. Ajouter au minimum la chargée d'enquête en administratrice, ou transférer le dépôt à une organisation GitHub Sciences Po si elle existe.
   - **Nom de domaine.** Si la DSI peut créer un sous-domaine `sciencespo.fr` pointant vers GitHub Pages (fichier `CNAME` à la racine), on obtient l'URL institutionnelle **sans rien changer au fonctionnement**. C'est la meilleure sortie : le confort de git, la crédibilité du domaine. À demander sans en faire un bloquant.
2. **Repli papier (indispensable si l'URL reste `github.io`)** — imprimer l'adresse mail en toutes lettres sous le QR. Double effet : le dispositif survit à une page inaccessible, et l'étudiant qui se méfie d'une URL non institutionnelle peut faire la démarche sans passer par la page. C'est ce qui rend le choix `github.io` acceptable.
3. **Mesure d'usage** — la page ne trace rien ; le volume réel se lit dans l'onglet `logs` du classeur de mapping.

## Prototype en ligne (25/08/2026)

- **URL** : https://mofy-scpo.github.io/barometre-qr/
- **QR** : `qr-barometre.png` (600 px, correction M) — pointe vers cette URL
- Statut : **prototype de démonstration**. Hébergement définitif à rebasculer sur un domaine institutionnel (Google Sites `sites.google.com/sciencespo.fr/…` ou page `sciencespo.fr`) **avant impression** des supports : un QR institutionnel qui atterrit sur `github.io` se lit comme du phishing.
