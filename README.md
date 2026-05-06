# 📊 IBM HR Analytics — Power BI Dashboard

> **Dataset** : [IBM HR Analytics — Kaggle](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset)  
> **Outil** : Power BI Desktop + Power BI Service  
> **Niveau** : Avancé — DAX, Power Query, Modélisation  

---

## 🎯 Objectif

Analyser les facteurs d'attrition RH et identifier les profils à risque de départ à partir d'un dataset de 1 470 employés et 35 colonnes.

---

## 🛠️ Power Query — Transformations appliquées

| # | Transformation | Détail |
|---|---|---|
| 1 | Remplacement valeurs | `Attrition` : Yes → Démissionné / No → Actif |
| 2 | Remplacement valeurs | `OverTime` : Yes → Oui / No → Non |
| 3 | Colonne conditionnelle | `Tranche_Age` : < 30 / 30-39 / 40-49 / 50+ |
| 4 | Colonne personnalisée | `Salaire_Annuel` = MonthlyIncome × 12 |
| 5 | Suppression colonnes | EmployeeCount · StandardHours · Over18 |

---

## 📐 Mesures DAX

### Mesures de base

```dax
Nb_Actifs = 
CALCULATE(COUNTROWS('HR'), 'HR'[Attrition] = "Actif")
```

```dax
Taux_Attrition = 
DIVIDE(
    CALCULATE(COUNTROWS('HR'), 'HR'[Attrition] = "Démissionné"),
    COUNTROWS('HR')
) * 100
```

```dax
Salaire_Annuel_Moyen = 
AVERAGE('HR'[Salaire_Annuel])
```

### CALCULATE + filtre simple

```dax
Salaire_Moyen_Partants = 
CALCULATE(
    AVERAGE('HR'[Salaire_Annuel]),
    'HR'[Attrition] = "Démissionné"
)
```

```dax
Moy_Anciennete_Partants = 
CALCULATE(
    AVERAGE('HR'[YearsAtCompany]),
    'HR'[Attrition] = "Démissionné"
)
```

```dax
Impact_Formation = 
CALCULATE(
    AVERAGE('HR'[TrainingTimesLastYear]),
    'HR'[Attrition] = "Actif"
)
```

### CALCULATE imbriqué

```dax
Attrition_OverTime = 
DIVIDE(
    CALCULATE(COUNTROWS('HR'), 'HR'[Attrition] = "Démissionné", 'HR'[OverTime] = "Oui"),
    CALCULATE(COUNTROWS('HR'), 'HR'[OverTime] = "Oui")
) * 100
```

### FILTER imbriqué

```dax
Profils_A_Risque = 
CALCULATE(
    COUNTROWS('HR'),
    FILTER('HR',
        'HR'[Attrition] = "Actif" &&
        'HR'[JobSatisfaction] <= 2 &&
        'HR'[OverTime] = "Oui"
    )
)
```

```dax
Attrition_Exp_Elevee = 
CALCULATE(
    COUNTROWS('HR'),
    FILTER('HR',
        'HR'[TotalWorkingYears] >= 10 &&
        'HR'[Attrition] = "Démissionné"
    )
)
```

### Mesure pondérée

```dax
Score_Bien_Etre = 
AVERAGE('HR'[JobSatisfaction]) * 0.40
+ AVERAGE('HR'[WorkLifeBalance]) * 0.35
+ AVERAGE('HR'[EnvironmentSatisfaction]) * 0.25
```

---

## 📊 Dashboard

*Captures à ajouter après publication sur Power BI Service*

| Page | Contenu |
|---|---|
| Page 1 | Vue générale RH — KPIs, attrition par département, OverTime |
| Page 2 | Facteurs d'attrition — Scatter plot, profils à risque, salaires |

---

## 💡 Insights

*À compléter après analyse du dashboard*

- Insight 1 :
- Insight 2 :
- Insight 3 :

---

## 🔗 Liens

- 🟡 [Power BI Service](#) — *lien à ajouter*
- 📁 [Dataset Kaggle](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset)
