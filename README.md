# **Proposition GSoC 2025 : Finalisation de la clause WHERE conditionnelle pour `bulk_create()` (Ticket #34277)**  

## **Contexte Technique**  

### **Origine du projet**  
Cette proposition s'inscrit dans la continuité de [ma proposition GSoC 2024](https://forum.djangoproject.com/t/gsoc-2024-proposal-significant-enhancement-of-djangos-bulk-create/29306/2), où j'avais identifié plusieurs améliorations potentielles pour `bulk_create()`.  
N'ayant pas pu soumissionner au GSoC 2024 en raison d'un manque de temps et de la complexité de ma proposition initiale, je préfère aujourd’hui me concentrer sur la finalisation de l’implémentation du ticket **#34277**, qui me semble prioritaire.  

### **Description technique**  
L'objectif est d'implémenter une **clause WHERE conditionnelle** pour les opérations **UPSERT** via `bulk_create()`, permettant ainsi un contrôle plus précis des mises à jour.  

#### **Exemples d'utilisation** :  
```python
Model.objects.bulk_create(
    updated_items,
    update_conflicts=True,
    update_fields=["name", "rank"],
    unique_fields=["number"],
    condition=Q(rank__gt=F("number") + 1) & ~Q(name__startswith="A") | Q(number__gt=3),  # Nouveau
)
```
```python
Model.objects.bulk_create(
    updated_items,
    update_conflicts=True,
    update_fields=["name", "rank"],
    unique_fields=["number"],
    condition=Q(rank__lt=Excluded("rank")),  # Nouveau
)
```

---

## **État d'Avancement**  

### **Travail existant (PR #17515)**  
✅ Implémentation de base pour **PostgreSQL** et **SQLite**  
✅ Introduction de la classe `Excluded`  
✅ Support des expressions `Q()` et `F()`  
📊 **14 fichiers modifiés** (+379/-10 lignes)  
💬 **186 commentaires traités**  

### **Retours clés des core developers**  
Merci à @kezabelle, @shangxiao, @bcail, @ngnpope, @felixxm et @sarahboyce pour leurs retours très constructifs.  

---

## **Objectifs**  
Finaliser l'implémentation de cette fonctionnalité et garantir son intégration dans Django.  

---

## **Plan de Travail (12 semaines)**  

### **Phase 1 : Préparation (3 semaines)**  
- Analyse approfondie des commentaires existants  
- Refactoring et optimisation de l'implémentation actuelle  

### **Phase 2 : Développement (9 semaines)**  
- Itérations de développement  
- Revue de code et intégration des retours  
- Tests et documentation  

---

## **Valeur Ajoutée**  

### **Pour les développeurs**  
#### **Exemple réel d'utilisation**  
```python
Product.objects.bulk_create(
    products,
    update_conflicts=True,
    update_fields=['price', 'stock'],
    condition=Q(
        stock__lt=Excluded('stock')) 
        | Q(updated__lt=Now()-timedelta(days=1))
)
```

### **Avantages concrets**  
✅ **Gain de précision** dans les mises à jour  
✅ **Réduction des opérations superflues**  
✅ **Meilleur contrôle des conflits**  

---

## **Engagement Communautaire**  

### **Contributions existantes**  
🔹 32 commits documentés  
🔹 86 réponses aux commentaires  
🔹 Couverture de tests atteignant **89% des cas d’usage**  

---

## **À Propos de Moi**  

👤 **Issaka Hama Barhamou**  
🔗 [LinkedIn](https://www.linkedin.com/in/barhamou-issaka-hama-90047b179/)  

### **Parcours**  
- **Développeur backend spécialisé en Django**  
- **Formation ALX Africa en ingénierie logicielle**  
- **Contributeur actif à Django depuis 2023**  

### **Motivation**  
💡 **Résoudre des problèmes concrets** de l'ORM Django  
💡 **Améliorer l’expérience développeur**  
💡 **Contribuer à un projet open-source majeur**  

