

# Names.Add, méthode (Excel)

Cette méthode définit un nouveau nom pour une plage de cellules.
 


## Syntaxe

 *expression*  . **Add**( ** *Name* **, ** *RefersTo* **, ** *Visible* **, ** *MacroType* **, ** *ShortcutKey* **, ** *Category* **, ** *NameLocal* **, ** *RefersToLocal* **, ** *CategoryLocal* **, ** *RefersToR1C1* **, ** *RefersToR1C1Local* ** )
 

 
 *expression*  Variable qui représente un objet **Names**.
 

 

### Paramètres



|**Nom**|**Obligatoire/Facultatif**|**Type de données**|**Description**|
|:-----|:-----|:-----|:-----|
| _Name_|Facultatif|**Variante**|Indique le texte, en anglais, à utiliser comme nom si le paramètre NameLocal n'est pas spécifié. Les noms ne doivent pas contenir d'espaces et ne peuvent pas avoir le format des références de cellule.|
| _RefersTo_|Facultatif|**Variante**|Décrit ce à quoi le nom fait référence, en anglais, en utilisant la notation de style A1, si les paramètres RefersToLocal, RefersToR1C1 et RefersToR1C1Local ne sont pas spécifiés. <BR/><BR/>**Remarque**<BR/>   **Nothing** est renvoyé si la référence n'existe pas. |
| _Visible_|Facultatif|**Variante**|**True** définit le nom comme visible. **False** définit le nom comme masqué (c'est-à-dire qu'il n'apparaît pas dans les boîtes de dialogue **Définir un nom**,  **Coller un nom** ou **Atteindre**. La valeur par défaut est  **True**.|
| _MacroType_|Facultatif|**Variante**| Type de macro, déterminé par une des valeurs suivantes : <BR/>1 - Fonction définie par l'utilisateur (procédure **Function** ) <BR/>2 - Macro (procédure **Sub** ) <BR/>3 ou omis - Aucune (le nom ne fait référence à aucune macro ni fonction définie par l'utilisateur)|
| _ShortcutKey_|Facultatif|**Variante**|Indique la touche de raccourci de la macro. Il doit s'agir d'une seule lettre, telle que « z » ou « Z ». Ne s'applique qu'aux macros de commande.|
| _Category_|Facultatif|**Variante**|Catégorie de la macro ou fonction si l'argument MacroType a la valeur 1 ou 2. La catégorie est utilisée dans l'Assistant Fonction. La référence à des catégories existantes s'effectue soit avec un nombre, en commençant par 1, soit avec un nom en anglais. Microsoft Office Excel 2007 crée une catégorie si la catégorie spécifiée n'existe pas.|
| _NameLocal_|Facultatif|**Variante**|Indique le texte localisé à utiliser comme nom si le paramètre Name n'est pas spécifié. Les noms ne doivent pas contenir d'espaces et ne peuvent pas avoir le format des références de cellule.|
| _RefersToLocal_|Facultatif|**Variante**|Décrit ce à quoi le nom fait référence, en texte localisé, en utilisant la notation de style A1, si les paramètres RefersTo, RefersToR1C1 et RefersToR1C1Local ne sont pas spécifiés.|
| _CategoryLocal_|Facultatif|**Variante**|Indique le texte localisé qui identifie la catégorie d'une fonction personnalisée si le paramètre Category n'est pas spécifié.|
| _RefersToR1C1_|Facultatif|**Variante**|Décrit ce à quoi le nom fait référence, en anglais, en utilisant la notation de style R1C1, si les paramètres RefersTo, RefersToLocal et RefersToR1C1Local ne sont pas spécifiés.|
| _RefersToR1C1Local_|Facultatif|**Variante**|Décrit ce à quoi le nom fait référence, en texte localisé, en utilisant la notation de style R1C1, si les paramètres RefersTo, RefersToLocal et RefersToR1C1 ne sont pas spécifiés.|

### Valeur renvoyée

Un objet  ** [Name](cfedb297-ac0d-dff0-99c7-6927cc5f31ed.md)** qui représente le nouveau nom.
 

 

## Exemple

Cet exemple montre comment définir un nouveau nom pour la plage A1:D3 située dans la feuille Sheet1 du classeur actif.
 

 

 **Remarque**   **Nothing** est renvoyé si la feuille Sheet1 n'existe pas.
 


```
Sub MakeRange() 
 
    ActiveWorkbook.Names.Add _ 
        Name:="tempRange", _ 
        RefersTo:="=Sheet1!$A$1:$D$3" 
 
End Sub
```


## Voir aussi


#### Concepts


 
 [Objet Names](ffecf89d-7bae-c470-8e37-608857a9de2a.md)
#### Autres ressources


 
 [Membres de l'objet Names](32c3c4d9-80fb-28c8-86e0-d504e3bfc0ba.md)