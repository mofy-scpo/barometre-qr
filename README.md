# Baromètre étudiant 2026 — page « recevoir mon lien »

Page d'atterrissage du **QR code** présenté par les ambassadeurs sur les campus. Elle explique en trois étapes comment récupérer son lien d'enquête personnel, et propose trois façons d'envoyer le mail déclencheur (Gmail, application mail, copie de l'adresse).

Dispositif complet : livrable **L9** (`~/enquete_barometre/livrables/L9_qr_code_lien_personnel.md`) · mission MANEO **M-2026050**.

## Ce que fait la page

- **« Ouvrir dans Gmail »** → `https://mail.google.com/a/sciencespo.fr/mail/?view=cm&fs=1&tf=1&to=…&su=…`
  Le segment `/a/sciencespo.fr/` **force le compte du domaine** si l'étudiant a plusieurs comptes Google connectés — c'est ce qui évite l'erreur la plus probable (envoyer depuis un Gmail personnel, et ne jamais recevoir de réponse). Sur mobile, ce lien ouvre l'application Gmail lorsqu'elle est installée ; sinon la version web.
- **« Ouvrir mon application mail »** → `mailto:` classique (Apple Mail, Outlook, autre).
- **Bouton « Copier »** → presse-papiers, avec repli `execCommand` pour les contextes non sécurisés.
- Bascule **FR / EN**, anglais par défaut si le navigateur n'est pas francophone.
- **Aucun cookie, aucun script tiers, aucune requête sortante.** Un seul fichier, 12 Ko.

Le `+` de l'adresse est encodé `%2B` dans l'URL Gmail (paramètre de requête) et laissé littéral dans le `mailto:` — c'est volontaire, l'inverse casse l'adresse.

## Déploiement (GitHub Pages)

```bash
cd ~/barometre-qr
git init && git add -A && git commit -m "Page QR Baromètre 2026"
gh repo create <compte>/barometre-qr --public --source=. --push
# puis : Settings → Pages → Source: main / (root)
```
URL obtenue : `https://<compte>.github.io/barometre-qr/`

## À tester avant impression du QR

- [ ] iOS + Gmail installé → le bouton Gmail ouvre l'app sur le bon compte
- [ ] iOS sans Gmail → bouton mailto (Apple Mail) fonctionnel
- [ ] Android + Gmail → idem
- [ ] Étudiant avec **deux comptes Google** connectés → vérifie que `/a/sciencespo.fr/` sélectionne bien le compte institutionnel
- [ ] Bouton Copier sur mobile (Safari et Chrome)
- [ ] Lisibilité en plein soleil, à bout de bras

## Points ouverts

1. **Hébergement définitif** — GitHub Pages est un domaine tiers ; pour une sollicitation institutionnelle, un domaine `sciencespo.fr` (ou `sites.google.com/sciencespo.fr/…`) inspire davantage confiance et résiste mieux au réflexe « c'est du phishing ». À arbitrer avant l'impression.
2. **Repli papier** — imprimer l'adresse en toutes lettres sur le support, sous le QR : si la page est inaccessible, le dispositif reste utilisable.
3. **Mesure d'usage** — la page ne trace rien ; le volume réel se lit dans l'onglet `logs` du classeur de mapping.

## Prototype en ligne (25/08/2026)

- **URL** : https://mofy-scpo.github.io/barometre-qr/
- **QR** : `qr-barometre.png` (600 px, correction M) — pointe vers cette URL
- Statut : **prototype de démonstration**. Hébergement définitif à rebasculer sur un domaine institutionnel (Google Sites `sites.google.com/sciencespo.fr/…` ou page `sciencespo.fr`) **avant impression** des supports : un QR institutionnel qui atterrit sur `github.io` se lit comme du phishing.
