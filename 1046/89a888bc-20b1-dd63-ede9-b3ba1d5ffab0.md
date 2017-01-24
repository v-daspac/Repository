

# Método Names.Add (Excel)

Define um novo nome para um intervalo de células.
 


## Sintaxe

 *expressão*  . **Add**( ** *Name* **, ** *RefersTo* **, ** *Visible* **, ** *MacroType* **, ** *ShortcutKey* **, ** *Category* **, ** *NameLocal* **, ** *RefersToLocal* **, ** *CategoryLocal* **, ** *RefersToR1C1* **, ** *RefersToR1C1Local* ** )
 

 
 *expressão*  Uma variável que representa um objeto **Names**.
 

 

### Parâmetros



|**Nome**|**Obrigatório/opcional**|**Tipo de dados**|**Descrição**|
|:-----|:-----|:-----|:-----|
| _Name_|Opcional|**Variant**|Especifica o texto, em inglês, a ser usado como nome se o parâmetro NameLocal não for especificado. Os nomes não podem conter espaços nem podem ser formatados como referências a células.|
| _RefersTo_|Opcional|**Variant**|Descreve a que se refere o nome, em inglês, usando a notação no estilo A1, se os parâmetros RefersToLocal, RefersToR1C1 e RefersToR1C1 não forem especificados. **Observação**   **Nothing** será retornado se a referência não existir. |
| _Visible_|Opcional|**Variant**|**True** especifica que o nome é definido como visível e **False** como oculto. Um nome oculto não aparece na caixa de diálogo **Definir Nome**,  **Colar Nome** ou **Ir para**. O valor padrão é  **True**.|
| _MacroType_|Opcional|**Variant**| O tipo de macro, determinado por um destes valores: 1 - Função definida pelo usuário (procedimento **Function** ) 2 - Macro (procedimento **Sub** ) 3 ou omitido - Nenhum (o nome não se refere a uma macro ou função definida pelo usuário)|
| _ShortcutKey_|Opcional|**Variant**|Especifica a tecla de atalho da macro. Deve ser uma única letra, como "z" ou "Z". Aplica-se somente a macros de comando.|
| _Category_|Opcional|**Variant**|A categoria da macro ou função se o argumento MacroType for 1 ou 2. A categoria é usada no Assistente de Função. Categorias existentes podem ser referidas por número, começando por 1, ou por nome, em inglês. O Microsoft Office Excel 2007 cria uma nova categoria quando a especificada não existe.|
| _NameLocal_|Opcional|**Variant**|Especifica o texto localizado a ser usado como nome se o parâmetro Name não for especificado. Os nomes não podem conter espaços nem podem ser formatados como referências a células.|
| _RefersToLocal_|Opcional|**Variant**|Descreve a que se refere o nome, no texto localizado, usando a notação no estilo A1, se os parâmetros RefersTo, RefersToR1C1 e RefersToR1C1Local não forem especificados.|
| _CategoryLocal_|Opcional|**Variant**|Especifica o texto localizado que identifica a categoria de uma função personalizada, se o parâmetro Category não for especificado.|
| _RefersToR1C1_|Opcional|**Variant**|Descreve a que se refere o nome, em inglês, usando a notação no estilo R1C1, se os parâmetros RefersTo, RefersToLocal e RefersToR1C1Local não forem especificados.|
| _RefersToR1C1Local_|Opcional|**Variant**|Descreve a que se refere o nome, no texto localizado, usando a notação no estilo R1C1, se os parâmetros RefersTo, RefersToLocal e RefersToR1C1 não forem especificados.|

### Valor retornado

Um objeto  ** [Name](cfedb297-ac0d-dff0-99c7-6927cc5f31ed.md)** que representa o novo nome.
 

 

## Exemplo

Este exemplo define um novo nome para o intervalo A1:D3 de Sheet1 na pasta de trabalho ativa.
 

 

 **Observação**   **Nothing** será retornado se Sheet1 não existir.
 


```
Sub MakeRange() 
 
    ActiveWorkbook.Names.Add _ 
        Name:="tempRange", _ 
        RefersTo:="=Sheet1!$A$1:$D$3" 
 
End Sub
```


## Ver também


#### Conceitos


 
 [Objeto Names](ffecf89d-7bae-c470-8e37-608857a9de2a.md)
#### Outros recursos


 
 [Membros do Objeto Names](32c3c4d9-80fb-28c8-86e0-d504e3bfc0ba.md)