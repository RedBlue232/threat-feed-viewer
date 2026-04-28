# feed-viewer

Page statique qui visualise le feed glissant 7 jours d'IPs malveillantes du
projet [threat-feed-publisher](https://github.com/RedBlue232/threat-feed-publisher).

🔗 **En ligne** : [feed.cyberdefense.blue](https://feed.cyberdefense.blue)

## Comment ça marche

Single-file HTML/JS vanilla, zéro dépendance. Au chargement, fetch les 3 feeds JSON (`feed-{all,crowdsec,suricata}-7d.json`) directement depuis le repo source via `raw.githubusercontent.com`. Affiche table filtrable, recherche, détail par IP avec liens lookup vers CrowdSec CTI, AbuseIPDB, VirusTotal, Shodan et GreyNoise.

UX inspirée de [beaconbeagle.com](https://beaconbeagle.com).

## Déploiement

GitHub Pages + custom domain via `CNAME`. Aucun build, aucune CI nécessaire.