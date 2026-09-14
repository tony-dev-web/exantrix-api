# Exantrix seller API

Collection Postman et spécification OpenAPI de l'API vendeur de la marketplace [Exantrix](https://exantrix.com) (impression 3D, DTF, textile personnalisé, flocage, découpe).

- **Documentation** : https://exantrix.com/extensions/api (FR) · https://exantrix.com/en/extensions/api (EN)
- **Jeton API** : généré dans votre espace vendeur, section API : https://exantrix.com/a2/vendeur

## Fichiers

| Fichier | Usage |
|---|---|
| `exantrix-api.postman_collection.json` | Collection Postman (v2.1) : compte, produits, stock, commandes, expédition, exemple de webhook reçu, tests automatiques |
| `exantrix-api.postman_environment.json` | Environnement Postman : `base` et `jeton` (secret) |
| `openapi.yaml` | Spécification OpenAPI 3.0 (importable dans Postman, Insomnia, Swagger UI, générateurs de clients) |

## Utilisation dans Postman

1. Import › fichiers `exantrix-api.postman_collection.json` et `exantrix-api.postman_environment.json`.
2. Sélectionnez l'environnement « Exantrix production » et renseignez `jeton`.
3. Lancez « Mon compte vendeur » : la réponse contient votre `vendeur_id` et votre statut.

## Webhook

Chaque commande payée est envoyée en POST JSON à l'URL de notification renseignée dans votre espace vendeur, avec l'en-tête `X-Exantrix-Signature: sha256=<HMAC-SHA256 hexadécimal du corps brut, clé = jeton>`. Vérifiez la signature avant tout traitement.

```python
import hashlib, hmac

def signature_valide(corps_brut: bytes, entete: str, jeton: str) -> bool:
    attendu = "sha256=" + hmac.new(jeton.encode(), corps_brut, hashlib.sha256).hexdigest()
    return hmac.compare_digest(attendu, entete)
```

## Extensions prêtes à l'emploi

- [WordPress / WooCommerce](https://github.com/tony-dev-web/exantrix-marketplace-wordpress)
- [PrestaShop](https://github.com/tony-dev-web/exantrix-marketplace-prestashop)
- [Shopify](https://github.com/tony-dev-web/exantrix-marketplace-shopify)
- [Magento 2](https://github.com/tony-dev-web/exantrix-marketplace-magento)
- [Drupal Commerce](https://github.com/tony-dev-web/exantrix-marketplace-drupal)

Licence MIT.
