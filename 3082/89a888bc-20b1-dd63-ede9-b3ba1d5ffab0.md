

# Método Names.Add (Excel)

Define un nombre nuevo para un rango de celdas.
 


## Sintaxis

 *expresión*  . **Add**( ** *Name* **, ** *RefersTo* **, ** *Visible* **, ** *MacroType* **, ** *ShortcutKey* **, ** *Category* **, ** *NameLocal* **, ** *RefersToLocal* **, ** *CategoryLocal* **, ** *RefersToR1C1* **, ** *RefersToR1C1Local* ** )
 

 
 *expresión*  Variable que representa un objeto **Names**.
 

 

### Parámetros



|**Nombre**|**Necesario/Opcional**|**Tipo de datos**|**Descripción**|
|:-----|:-----|:-----|:-----|
| _Name_|Opcional|**Variant**|Especifica el texto que se usa como nombre (en el lenguaje de la macro) si no se especifica el parámetro NameLocal. Los nombres no pueden incluir espacios ni pueden tener formato de referencias de celdas.|
| _RefersTo_|Opcional|**Variant**|Describe el elemento al cual se refiere el nombre (en el lenguaje de la macro) con la notación de estilo A1, si no se especifican los parámetros RefersToLocal, RefersToR1C1 y RefersToR1C1Local. <BR/><BR/>**Nota**<BR/>  Se devuelve  **Nothing** si la referencia no existe. |
| _Visible_|Opcional|**Variant**|Si es  **True**, el nombre se define como visible. Si es **False**, el nombre se define como oculto (es decir, no aparece en los cuadros de diálogo **Definir nombre**,  **Pegar nombre** ni **Ir a**). El valor predeterminado es  **True**.|
| _MacroType_|Opcional|**Variant**| Tipo de macro, determinado por uno de los valores siguientes: <BR/>1: función definida por el usuario (procedimiento **Function** ) <BR/>2: macro (procedimiento **Sub** ) <BR/>3 o sin especificar: ninguno (es decir, el nombre no se refiere a una función definida por el usuario o una macro)|
| _ShortcutKey_|Opcional|**Variant**|Tecla de método abreviado de la macro. Debe ser una sola letra, por ejemplo, "z" o "Z". Únicamente se aplica a macros de comando.|
| _Category_|Opcional|**Variant**|Categoría de la macro o función si el argumento MacroType es 1 o 2. La categoría se usa en el Asistente para funciones. Puede hacer referencia a las categorías existentes por número (a partir de 1) o por nombre (en el lenguaje de la macro). Microsoft Office Excel 2007 crea una nueva categoría si la especificada no existe.|
| _NameLocal_|Opcional|**Variant**|Especifica el texto (localizado) que se usa como nombre si no se especifica el parámetro Name. Los nombres no pueden incluir espacios ni pueden tener formato de referencias de celdas.|
| _RefersToLocal_|Opcional|**Variant**|Describe el elemento al cual se refiere el nombre, en texto localizado, con la notación de estilo A1, si no se especifican los parámetros RefersTo, RefersToR1C1 y RefersToR1C1Local.|
| _CategoryLocal_|Opcional|**Variant**|Especifica el texto localizado que identifica la categoría de una función personalizada si no se especifica el parámetro Category.|
| _RefersToR1C1_|Opcional|**Variant**|Describe el elemento al cual hace referencia el nombre (en el lenguaje de la macro) con la notación de estilo R1C1 si no se especifican los parámetros RefersTo, RefersToLocal y RefersToR1C1Local.|
| _RefersToR1C1Local_|Opcional|**Variant**|Describe el elemento al cual hace referencia el nombre (en texto localizado) con la notación de estilo R1C1 si no se especifican los parámetros RefersTo, RefersToLocal y RefersToR1C1.|

### Valor devuelto

Objeto  ** [Name](cfedb297-ac0d-dff0-99c7-6927cc5f31ed.md)** que representa el nuevo nombre.
 

 

## Ejemplo

En este ejemplo se define un nombre nuevo para el rango A1:D3 de Sheet1 del libro activo.
 

 

 **Nota**  Se devuelve  **Nothing** si Sheet1 no existe.
 


```
Sub MakeRange() 
 
    ActiveWorkbook.Names.Add _ 
        Name:="tempRange", _ 
        RefersTo:="=Sheet1!$A$1:$D$3" 
 
End Sub
```


## Vea también


#### Conceptos


 
 [Objeto Names](ffecf89d-7bae-c470-8e37-608857a9de2a.md)
#### Otros recursos


 
 [Miembros del objeto Names](32c3c4d9-80fb-28c8-86e0-d504e3bfc0ba.md)