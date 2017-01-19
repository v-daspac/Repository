
# Report.Requery-Methode (Access)

Mit der  **Requery** -Methode werden die dem angegebenen Bericht zugrunde liegenden Daten aktualisiert, indem die Datenquelle des Steuerelements erneut abgefragt wird.
 


## Syntax

 *Ausdruck*  . **Requery**
 

 
 *Ausdruck*  Eine Variable, die ein **Report** -Objekt darstellt.
 

 

## Hinweise

Mit dieser Methode stellen Sie sicher, dass im Formular oder Steuerelement die aktuellen Daten angezeigt werden.
 

 
Die Methode  **Requery** führt einen der folgenden Vorgänge aus:
 

 

- Erneutes Ausführen der Abfrage, auf der der Bericht basiert.
    
 
- Aktualisieren der angezeigten Datensätze, die auf den Änderungen der  **Filter** -Eigenschaft des Berichts basieren.
    
 
Wenn Sie das durch Ausdruck angegebene Objekt weglassen, führt die  **Requery** -Methode eine Abfrage der zugrunde liegenden Datenquelle des Berichts aus, der den Fokus besitzt. Wenn das Steuerelement, das den Fokus hat, eine Datensatzquelle oder Datensatzherkunft besitzt, wird diese erneut abgefragt. Andernfalls werden die Daten des Steuerelements einfach aktualisiert.
 

 

 **Hinweis**   Die **Requery** -Methode aktualisiert die einem Formular oder Steuerelement zugrunde liegenden Daten, um Datensätze anzuzeigen, die neu sind oder seit der letzten Abfrage aus der Datensatzquelle gelöscht wurden. Die **Refresh** -Methode zeigt nur Änderungen, die an den aktuellen Datensätzen vorgenommen wurden, und keine neuen oder in der Datensatzquelle gelöschten Datensätze. Die **Repaint** -Methode aktualisiert lediglich das angegebene Formular und dessen Steuerelemente. Die **Requery** -Methode gibt die Kontrolle nicht an das Betriebssystem weiter, um es Windows so zu ermöglichen, die Verarbeitung von Nachrichten fortzusetzen. Verwenden Sie die **DoEvents** -Funktion, falls Sie die Kontrolle temporär an das Betriebssystem übergeben möchten. Die **Requery** -Methode ist schneller als die Requery-Aktion. Wenn Sie die "Requery"-Aktion verwenden, schließt Microsoft Access zuerst die Abfrage und lädt sie dann neu aus der Datenbank. Wenn Sie die **Requery** -Methode verwenden, führt Microsoft Access die Abfrage erneut aus, ohne sie vorher zu schließen und neu zu laden.
 


## Siehe auch


#### Konzepte


 
 [Report-Objekt](6f77c1b4-a9ce-7caa-204c-fe0755c6f9df.md)
#### Weitere Ressourcen


 
 [Report-Objektelemente](73370a33-1ca0-da4d-9e36-88011bc2b93e.md)