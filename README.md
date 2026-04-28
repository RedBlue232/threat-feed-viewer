# Threat feed viewer

Page statique autonome (HTML + CSS + JS vanilla, aucune dépendance) qui consomme
les feeds JSON publiés sur GitHub et offre :

- table filtrable par scope (`all` / `crowdsec` / `suricata`)
- recherche texte unifiée (IP, ASN, scenario, tag)
- filtres tags actifs (cloud, scanner, tor, ASN)
- tri sur toutes les colonnes
- drawer latéral détail (scenarios + payloads + actions copy)
- dark mode automatique selon `prefers-color-scheme`
- liens de téléchargement direct des feeds bruts (txt, json)

## Source des données

Les feeds sont tirés à chaque chargement depuis :

```
https://raw.githubusercontent.com/RedBlue232/threat-feed-publisher/main/feeds/feed-{scope}-7d.json
```

(les 3 scopes en parallèle). `raw.githubusercontent.com` expose les fichiers
avec CORS permissif, donc le sous-domaine `feed.cyberdefense.blue` peut les
fetcher sans proxy.

Pour pointer vers un fork ou une autre branche, change `REPO` et `BRANCH` en
tête du `<script>`.

## Déploiement (site Hugo)

1. Copier `index.html` dans `static/threat-feed/` (ou autre sous-chemin) du
   repo Hugo.
2. Build Hugo + push → fichier servi tel quel à
   `https://cyberdefense.blue/threat-feed/`.

## Sous-domaine `feed.cyberdefense.blue`

Deux options selon ton hébergement :

**Option A — sous-domaine séparé pointant vers GitHub Pages**

Si ton site Hugo est déjà sur GitHub Pages avec custom domain, le plus simple
est de créer un second repo Pages dédié au viewer (ex.
`feed-cyberdefense-blue`) avec juste `index.html` + un fichier `CNAME`
contenant `feed.cyberdefense.blue`. Puis DNS :

```
CNAME feed.cyberdefense.blue → <ton-user>.github.io
```

**Option B — sous-domaine virtuel sur ton hébergeur (Cloudflare/Netlify…)**

Configurer un sous-domaine qui sert les fichiers d'un sous-dossier précis.
Variable selon le provider.

## Configuration

Pour cibler un autre repo / branche, éditer en haut du `<script>` :

```js
const REPO   = "RedBlue232/threat-feed-publisher";
const BRANCH = "main";
```

## Limites connues

- Pas de pagination : ok jusqu'à ~5 000 IPs, au-delà passer en virtual scroll.
- Pas de cache local : chaque visite refetch les 3 JSON (≤ 50 KB chacun).
- Pas de geoIP : si tu veux une carte plus tard, il faudra un service côté
  client (ip-api.com avec rate-limit, ou MaxMind embarqué).
