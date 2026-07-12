# Segmentation des clients — Olist (e-commerce)

Segmentation non supervisée des clients d'Olist, marketplace e-commerce brésilienne, pour une utilisation opérationnelle par l'équipe Marketing.

🔗 Repo : [github.com/Vortax1247/Segmentez-des-clients-dun-site-e-commerce](https://github.com/Vortax1247/Segmentez-des-clients-dun-site-e-commerce)

## Contexte

Olist propose une solution de vente sur marketplaces en ligne au Brésil. L'équipe Marketing souhaite une segmentation de ses clients, actionnable au quotidien pour ses campagnes de communication, permettant a minima de distinguer les bons et moins bons clients en termes de commandes et de satisfaction.

**Contraintes spécifiques du projet** :
- Peu de données disponibles pour des raisons de confidentialité, avec un déséquilibre fort : seuls 3 % des clients du fichier ont réalisé plusieurs commandes.
- La segmentation doit couvrir l'ensemble des clients, pas seulement les clients récurrents.
- Le code doit respecter la convention **PEP8** pour être réutilisable par les équipes internes d'Olist.
- Livrable additionnel : une recommandation de fréquence de mise à jour du modèle de segmentation, pour dimensionner un contrat de maintenance.

## Données

- Source : base de données anonymisée fournie par Olist — historique de commandes, produits achetés, avis de satisfaction, localisation des clients, depuis janvier 2017.

> ⚠️ **Données non incluses dans le repo**, pour des raisons de confidentialité (données clients fournies par Olist dans le cadre de la formation). Le dataset public équivalent (Olist Brazilian E-Commerce dataset) est disponible sur Kaggle pour reproduire l'analyse.

## Méthodologie

1. **Analyse exploratoire** (`HUMMEL_Adrien_1_notebook_exploration_032023.ipynb`, non nettoyé pour montrer la démarche) — compréhension du jeu de données, mise en évidence du déséquilibre entre clients mono-commande et multi-commandes.
2. **Feature Engineering** — construction des indicateurs par client :
   - Analyse **RFM** (Récence, Fréquence, Montant)
   - Variables de satisfaction (avis clients)
   - Réduction de dimension par **ACP**
3. **Modélisation** (`HUMMEL_Adrien_2_notebook_essais_032023.ipynb`, non nettoyé) — essais de plusieurs approches de clustering non supervisé, avec optimisation des hyperparamètres (ex. nombre de clusters via méthode du coude / score de silhouette). Code conforme **PEP8**.
4. **Simulation de stabilité temporelle** (`HUMMEL_Adrien_3_notebook_simulation_032023.ipynb`) — étude de l'évolution des segments dans le temps, pour déterminer la fréquence à laquelle le modèle doit être ré-entraîné afin de rester pertinent (base du devis de contrat de maintenance).
5. **Restitution métier** (`HUMMEL_Adrien_4_presentation_032023_2.pdf`) — description actionable de chaque segment, pensée pour une équipe Marketing non technique.

## Résultats

**9 segments identifiés**, chacun caractérisé par un profil comportemental dominant :

| Segment | Caractéristique dominante |
|---|---|
| Classique_1_commande_récente | Client classique, commande récente |
| Plusieurs_commandes | Client récurrent, plusieurs commandes |
| Retard_livraison_mauvais_avis | Retard de livraison associé à un mauvais avis |
| Classique_3_peu_intéressant | Client classique, peu engageant pour le marketing |
| Classique_2_poids_conséquents | Client classique, produits à poids conséquent |
| Plusieurs_types_paiements_beaucoup_produits | Diversité de moyens de paiement, gros volume de produits |
| Aime_les_photos | Sensible aux photos produit |
| Frais_livraison_importants_aime_description | Frais de livraison élevés, sensible à la description produit |
| Validation_longue | Délai de validation de commande long |

**Actionnable pour le Marketing** — cette segmentation permet de cibler des campagnes différenciées, par exemple :
- Mise en avant de nouveaux produits (segment sensible aux descriptions)
- Réduction sur les commandes à grande quantité de produits
- Remboursement des frais de transport à partir d'un certain montant de commande
- Mailing avec photos pour les segments sensibles au visuel

**Fréquence de mise à jour recommandée : tous les 2 mois.** L'étude de stabilité (simulation sur plusieurs découpages en 8 à 11 clusters) montre une dégradation importante de la cohérence des segments dès le deuxième mois suivant l'entraînement du modèle — au-delà, la segmentation devient trop instable pour rester exploitable en l'état, ce qui fonde la recommandation de fréquence pour le contrat de maintenance.

## Structure du repo

```
├── HUMMEL_Adrien_1_notebook_exploration_032023.ipynb    # analyse exploratoire (non nettoyé)
├── HUMMEL_Adrien_2_notebook_essais_032023.ipynb          # essais de modélisation (non nettoyé)
├── HUMMEL_Adrien_3_notebook_simulation_032023.ipynb      # simulation fréquence de maintenance
├── HUMMEL_Adrien_4_presentation_032023_2.pdf             # support de présentation
└── README.md
```

## Lancer le projet

```bash
git clone https://github.com/Vortax1247/Segmentez-des-clients-dun-site-e-commerce.git
cd Segmentez-des-clients-dun-site-e-commerce
pip install -r requirements.txt   # scikit-learn, pandas, numpy, matplotlib...
jupyter notebook HUMMEL_Adrien_1_notebook_exploration_032023.ipynb
```

## Pistes d'amélioration

- tester d'autres algorithmes de clustering (DBSCAN, clustering hiérarchique) en comparaison
- affiner la segmentation des clients mono-commande (97% du fichier), sous-exploités par une segmentation classique basée sur la récurrence
