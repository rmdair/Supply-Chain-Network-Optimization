<h1 align="center">Optimisation d'un réseau logistique</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Language-AMPL-d2b48c?style=for-the-badge&logo=codeforces&logoColor=white" alt="AMPL">
  <img src="https://img.shields.io/badge/Solver-CPLEX-darkgreen?style=for-the-badge&logo=ibm&logoColor=white" alt="Solver">
  <img src="https://img.shields.io/badge/License-MIT-blue?style=for-the-badge" alt="License">
</p>

<p align="center">
  <img src="assets/banner.webp" alt="Banner" width="60%">
</p>

Dans un contexte où la logistique est un enjeu stratégique pour les entreprises, l’optimisation des coûts de transport devient un levier essentiel pour améliorer la rentabilité et l’efficacité des flux de distribution. Ce projet propose une modélisation détaillée d’un problème logistique à l’aide du langage **AMPL**, avec plusieurs scénarios d’optimisation pour une entreprise allemande cherchant à approvisionner ses clients tout en minimisant ses coûts.

---

## Le langage AMPL  
AMPL (**A Mathematical Programming Language**) est un langage de modélisation utilisé pour résoudre des problèmes d’optimisation linéaire et non linéaire. Il permet une séparation claire entre :
- **Les données** (fichiers `.dat`) ;
- **Le modèle mathématique** (fichiers `.mod`) ;
- **Les commandes de résolution** (fichiers `.run`).  

Grâce à cette structure modulaire, AMPL facilite l'expérimentation avec différents solveurs comme **CPLEX** pour trouver des solutions optimales.


## Objectifs du projet  
L’étude explore différentes approches d’optimisation, incluant :
- **Minimisation des coûts de transport** entre usines, dépôts et clients ;
- **Comparaison des stratégies de distribution**, avec ou sans entrepôts intermédiaires ;
- **Intégration des préférences clients** pour adapter le réseau logistique à leurs exigences ;
- **Réorganisation du réseau de dépôts**, en étudiant l’ouverture, l’extension ou la fermeture de certaines infrastructures.

### Visualisation du réseau

Voici la configuration géographique des usines et des dépôts, avec les flux de distribution réalisables de l'entreprise :

<p align="center">
  <img src="assets/logistics_network_design.webp" alt="Réseau de transport de l'entreprise" width="50%">
</p>


## Ressources   
Un rapport présente une analyse approfondie du problème, avec :
- La **formulation mathématique** complète sous AMPL ;
- Les **résultats des simulations**, illustrés par des tableaux et diagrammes de flux ;
- Les **interprétations** sur les différentes stratégies logistiques.

Consulter le rapport : [Optimisation d'un réseau logistique (PDF)](docs/Optimisation-Reseau-Logistique-AMPL.pdf)

 
## Formulation mathématique

Le modèle est formulé comme un problème de transport multi-étapes. L'objectif est de minimiser le coût total de distribution tout en respectant les capacités logistiques et les demandes clients.

### Fonction objectif

L'objectif est de minimiser la fonction $Z$ représentant la somme des coûts sur les trois types de routes possibles (Usine $\to$ Client, Usine $\to$ Dépôt, Dépôt $\to$ Client) :

$$
\begin{aligned}
\min Z = \sum_{u \in U} \sum_{c \in C} X_{uc} C_{uc} + \sum_{u \in U} \sum_{d \in D} X_{ud} C_{ud} + \sum_{d \in D} \sum_{c \in C} X_{dc} C_{dc}
\end{aligned}
$$

Où :
* $X_{ij}$ : Quantité transportée entre le point $i$ et le point $j$ (en tonnes).
* $C_{ij}$ : Coût unitaire de transport sur l'arc $(i, j)$ (en euros/tonne).
 
### Contraintes principales

* **Capacité des Usines ($U$) :** Le flux total sortant ne doit pas dépasser la capacité de production.
$$\sum_{c \in C} X_{uc} + \sum_{d \in D} X_{ud} \le \text{capa}_u, \quad \forall u \in U$$

* **Conservation des Flux (Dépôts $D$) :** Tout ce qui entre dans un dépôt doit en ressortir.
$$\sum_{u \in U} X_{ud} = \sum_{c \in C} X_{dc}, \quad \forall d \in D$$

* **Satisfaction de la Demande (Clients $C$) :** La somme des livraisons directes et via dépôts doit égaler le besoin du client.
$$\sum_{u \in U} X_{uc} + \sum_{d \in D} X_{dc} = \text{besoin}_c, \quad \forall c \in C$$


## Code et modélisation  
Le projet est structuré en six fichiers de code (accessibles via [ce lien](src/)) :
- **`probleme_transport_partie1.mod`** : Modèle AMPL de base avec les dépôts initiaux ;
- **`probleme_transport_partie1.dat`** : Données associées aux coûts de transport, capacités, demandes et préférences des clients ;
- **`probleme_transport_partie1.run`** : Commandes pour résoudre le modèle avec CPLEX ;
- **`probleme_transport_partie2.mod`** : Modèle AMPL intégrant les nouvelles options de dépôts ;
- **`probleme_transport_partie2.dat`** : Données mises à jour incluant les coûts d’ouverture, de fermeture et d'extension de dépôts ;
- **`probleme_transport_partie2.run`** : Commandes pour résoudre le nouveau modèle.

Les données utilisées dans ce projet ne proviennent pas d’un cas réel d’entreprise. Elles ont été générées de manière fictive mais cohérente, en s’appuyant sur des critères logistiques crédibles tels que les distances géographiques entre villes allemandes, des estimations réalistes de capacités, etc.


## Installation en local

Assurez-vous d'avoir **AMPL** et le solveur **CPLEX** installés.

Pour explorer ce projet localement :

```bash
# Cloner le dépôt
git clone https://github.com/rmdair/Supply-Chain-Network-Optimization.git

# Accéder au dossier du projet
cd Supply-Chain-Network-Optimization
```


## Référence
- AMPL Documentation officielle - *A Modeling Language for Mathematical Programming*  
[Lire le guide complet](https://ampl.com/wp-content/uploads/BOOK.pdf)
