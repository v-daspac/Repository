

# Names.Add-Methode (Excel)

Legt einen neuen Namen für einen Zellbereich fest.
 


## Syntax

 *Ausdruck*  . **Add**( ** *Name* **, ** *RefersTo* **, ** *Visible* **, ** *MacroType* **, ** *ShortcutKey* **, ** *Category* **, ** *NameLocal* **, ** *RefersToLocal* **, ** *CategoryLocal* **, ** *RefersToR1C1* **, ** *RefersToR1C1Local* ** )
 

 
 *Ausdruck*  Eine Variable, die ein **Names** -Objekt darstellt.
 

 

### Parameter



|**Name**|**Erforderlich/Optional**|**Datentyp**|**Beschreibung**|
|:-----|:-----|:-----|:-----|
| _Name_|Optional|**Variant**|Gibt den Text (in Englisch) an, der als Name zu verwenden ist, wenn der NameLocal-Parameter nicht angegeben wurde. Namen dürfen keine Leerzeichen enthalten und nicht als Zellbezüge formatiert werden.|
| _RefersTo_|Optional|**Variant**|Beschreibt, worauf der Name sich bezeiht, (in Englisch mit A1-Schreibweise), wenn die Parameter RefersToLocal, RefersToR1C1 und RefersToR1C1Local nicht angegeben sind. <BR/><BR/>**Hinweis**<BR/>   **Nothing** wird zurückgegeben, wenn die Referenz nicht vorhanden ist. |
| _Visible_|Optional|**Variant**|Mit  **True** wird der Name als sichtbar definiert. Mit **False** wird er als ausgeblendet definiert. Ein ausgeblendeter Name wird in den Dialogfeldern **Namen definieren**,  **Namen einfügen** oder **Gehe zu** nicht angezeigt. Der Standardwert ist **True**.|
| _MacroType_|Optional|**Variant**| Der Makrotyp, der von einem der folgenden Werte bestimmt wird: <BR/>1 - benutzerdefinierte Funktion ( **Function** -Prozedur) <BR/>2 - Makro ( **Unter** prozedur) <BR/>3 oder nicht angegeben - None (der Name bezieht sich nicht auf eine benutzerdefinierte Funktion oder ein Makro)|
| _ShortcutKey_|Optional|**Variant**|Gibt die Tastenkombination des Makros an. Es muss sich um einen einzelnen Buchstaben, z. B. "z" oder "Z" handeln. Dieses Argument gilt nur für Befehlsmakros.|
| _Category_|Optional|**Variant**|Die Kategorie des Makros oder der Funktion, wenn das MacroType-Argument gleich "1" oder "2" ist. Die Kategorie wird im Funktions-Assistenten verwendet. Der Bezug auf bestehende Kategorien kann in Form einer Zahl (beginnend mit 1) oder eines Namens (in Englisch) hergestellt werden. In Microsoft Office Excel 2007 wird eine neue Kategorie erstellt, wenn die angegebene Kategorie nicht vorhanden ist.|
| _NameLocal_|Optional|**Variant**|Gibt den lokalisierten Text an, der als Name zu verwenden ist, wenn der Name-Parameter nicht angegeben wurde. Namen dürfen keine Leerzeichen enthalten und nicht als Zellbezüge formatiert werden.|
| _RefersToLocal_|Optional|**Variant**|Beschreibt, worauf sich der Name bezieht (in lokalisiertem Text, unter Verwendung der A1-Schreibweise), wenn die Parameter RefersTo, RefersToR1C1 und RefersToR1C1Local nicht angegeben wurden.|
| _CategoryLocal_|Optional|**Variant**|Gibt den lokalisierten Text an, der die Kategorie einer benutzerdefinierten Funktion identifiziert, wenn der Category-Parameter nicht angegeben wurde.|
| _RefersToR1C1_|Optional|**Variant**|Beschreibt, worauf sich der Name bezieht (in Englisch, unter Verwendung der Z1S1-Schreibweise), wenn die Parameter RefersTo, RefersToLocal und RefersToR1C1Local nicht angegeben wurden.|
| _RefersToR1C1Local_|Optional|**Variant**|Beschreibt, worauf sich der Name bezieht (in lokalisiertem Text, unter Verwendung der Z1S1-Schreibweise), wenn die Parameter RefersTo, RefersToLocal und RefersToR1C1 nicht angegeben wurden.|

### Rückgabewert

Ein  ** [Name](cfedb297-ac0d-dff0-99c7-6927cc5f31ed.md)** -Objekt, das den neuen Namen darstellt.
 

 

## Beispiel

In diesem Beispiel wird ein neuer Name für den Bereich A1:D3 auf Sheet1 in der aktiven Arbeitsmappe definiert.
 

 

 **Hinweis**   **Nothing** wird zurückgegeben, wenn "Sheet1" nicht vorhanden ist.
 


```
Sub MakeRange() 
 
    ActiveWorkbook.Names.Add _ 
        Name:="tempRange", _ 
        RefersTo:="=Sheet1!$A$1:$D$3" 
 
End Sub
```


## Siehe auch


#### Konzepte


 
 [Names-Objekt](ffecf89d-7bae-c470-8e37-608857a9de2a.md)
#### Weitere Ressourcen


 
 [Names-Objektelemente](32c3c4d9-80fb-28c8-86e0-d504e3bfc0ba.md)