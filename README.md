# **GSoC 2025 Proposal: Finalizing the Conditional WHERE Clause for `bulk_create()` (Ticket #34277)**  

## **Technical Context**  

### **Project Background**  
This proposal continues the work from [my GSoC 2024 proposal](https://forum.djangoproject.com/t/gsoc-2024-proposal-significant-enhancement-of-djangos-bulk-create/29306/2), where I identified several potential improvements for `bulk_create()`.  
Since I was unable to submit my proposal for GSoC 2024 due to time constraints and the complexity of my initial idea, I now prefer to focus on finalizing the implementation of **ticket #34277**, which I consider a priority.  

### **Technical Description**  
The goal is to implement a **conditional WHERE clause** for **UPSERT** operations via `bulk_create()`, allowing for more precise control over updates.  

#### **Usage Examples**:  
```python
Model.objects.bulk_create(
    updated_items,
    update_conflicts=True,
    update_fields=["name", "rank"],
    unique_fields=["number"],
    condition=Q(rank__gt=F("number") + 1) & ~Q(name__startswith="A") | Q(number__gt=3),  # New
)
```
```python
Model.objects.bulk_create(
    updated_items,
    update_conflicts=True,
    update_fields=["name", "rank"],
    unique_fields=["number"],
    condition=Q(rank__lt=Excluded("rank")),  # New
)
```

---

## **Current Progress**  

### **Existing Work (PR #17515)**  
✅ Basic implementation for **PostgreSQL** and **SQLite**  
✅ Introduction of the `Excluded` class  
✅ Support for `Q()` and `F()` expressions  
📊 **14 files modified** (+379/-10 lines)  
💬 **186 comments addressed**  

### **Key Feedback from Core Developers**  
Special thanks to @kezabelle, @shangxiao, @bcail, @ngnpope, @felixxm, @sarahboyce, and @nessita for their valuable feedback.  

---

## **Objectives**  
Finalize the implementation of this feature and ensure its integration into Django.  

---

## **Work Plan (12 Weeks)**  

### **Phase 1: Preparation (3 Weeks)**  
- In-depth analysis of existing comments  
- Refactoring and optimization of the current implementation  

### **Phase 2: Development (9 Weeks)**  
- Iterative development  
- Code review and integration of feedback  
- Testing and documentation  

---

## **Added Value**  

### **For Developers**  
#### **Real-world Usage Example**  
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

### **Key Benefits**  
✅ **More precise updates**  
✅ **Reduction of unnecessary operations**  
✅ **Better conflict management**  

---

## **Community Engagement**  

### **Existing Contributions**  
🔹 32 documented commits  
🔹 86 responses to comments  
🔹 Test coverage reaching **89% of use cases**  

---

## **About Me**  

👤 **Issaka Hama Barhamou**  
🔗 [LinkedIn](https://www.linkedin.com/in/barhamou-issaka-hama-90047b179/)  

### **Background**  
- **Backend developer specializing in Django**  
- **ALX Africa Software Engineering Training**  
- **Active Django contributor since 2023**  

### **Motivation**  
💡 **Solving real-world ORM issues** in Django  
💡 **Enhancing the developer experience**  
💡 **Contributing to a major open-source project**  