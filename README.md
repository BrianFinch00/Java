Wir haben im Kurs mehrere Anwendungen programmiert, die es ermöglichen, Elemente in eine Liste hinzuzufügen oder aus der Liste zu entfernen. Jetzt geht es darum, eine solche Anwendung so zu erweitern, dass der Nutzer
- die Elemente der Liste auch bearbeiten kann
- die Reihenfolge der Elemente ändern kann.

Die Anwendung heißt "Diätplan" (VORSICHT! Kein ä in Ordner-, Projekt- oder Klassennamen!). Als Elemente der Liste verwaltet sie Gerichte. Jedes Gericht hat einen Namen, Kalorien (ein int) und ob es Vitamine enthält oder nicht (ein boolean).

Vorbereitung: Projekt, Datenklasse, Listen, Hinzufügen-Formular
Legen Sie das Projekt an, mit Package, Hauptklasse (mit main()), Fenster, Datenklasse (Gericht) mit Konstruktor und toString(), und Liste (DefaultListModel + JList).
Zum Testen, fügen Sie per Programmierung einige Elemente in die Liste hinzu.

Programmieren Sie ein Hinzufügen-Formular: ein JTextField für den Namen, ein JSpinner für die Kalorien, eine JCheckBox für die Vitamine, einen "Hinzufügen"-Button, und die entsprechende Funktion, die die Eingaben aus den Eingabeelementen holt, sie ggf. prüft, und ein neues Element in die Liste hinzufügt.

Sie dürfen den WindowBuilder benutzen, müssen aber nicht.

Sie dürfen ein Beispiel aus dem Kurs kopieren, aber passen Sie sorgfältig ALLE Namen an! (Rechte Maustaste auf eine Definition > Refactor > Rename). Sie werden dabei eine Variable in die Datenklasse und eine JCheckBox ins Fenster für die Vitamine hinzufügen müssen.

Testen Sie sorgfältig, zippen Sie den src-Ordner zusammen und laden Sie das .zip hoch.

Reihenfolgen-Editor
Es geht darum, zwei Buttons zu programmieren, mit den der Nutzer das markierte Element der Liste nach oben bzw. nach unten schieben kann, um die Reihenfolge zu ändern.

Fügen Sie unter die Liste zwei JButtons "nach oben" und "nach unten" hinzu. Die beiden rufen die selbe Funktion bewegen(int richtung) auf; bei "nach oben" ist die Richtung -1 und bei "nach unten" ist die Richtung 1.

Programmieren Sie die Funktion bewegen(). Sie bekommt die Bewegungsrichtung als Parameter (ein int).
Sie holt von der JList den markierten Index. Ist dieser Index nicht OK (<0 oder >= Anzahl Elemente), so wird die Funktion mit return; abgebrochen.
Sie rechnet den Ziel-Index der Bewegung: markierten Index + Richtung. Ist dieser Ziel-Index nicht OK , so wird auch die Funktion abgebrochen.
Sie tauscht die Elemente des ListModels mit diesen beiden Indexen um. Sie benutzt dafür die Methode .set(index,Element) des ListModels.
Dann teilt sie der JList den neuen markierten Index mit (mit ...setSelectedIndex()).

Testen Sie sorgfältig, zippen Sie den src-Ordner zusammen und laden Sie das .zip hoch.

Bearbeiten-Funktion
Fügen Sie auf die JList einen MouseListener hinzu, der auf Doppelklick reagiert:

if(e.getClickCount()>1) {
  .... falls ein Element markiert ist, es bearbeiten ...
}
Programmieren Sie die Funktion bearbeiten(). Sie bekommt ein Gericht-Objekt als Parameter, speichert es in eine globale Variable mit dem Namen "inBearbeitung", kopiert seine Daten in die Eingabefelder, und ändert den Text des Buttons so, dass er jetzt "Speichern" lautet (dafür muss die JButton-Variable global gemacht werden).

Mit welchem Wert muss die Variable inBearbeitung initialisiert werden (bevor der Nutzer ein Gericht doppelklickt)?

Ändern Sie die Hinzufügen-Funktion so:
- Sie heißt nicht mehr "hinzufuegen()" sondern "speichern()". Bennenen Sie auch die Variable des JButtons um.
- Sie holt die Eingaben aus den Eingabefeldern und prüft sie wie zuvor (nichts zu ändern).
- Falls inBearbeitung kein Gericht enthält, fügt sie wie zuvor ein neues Gericht in die Liste hinzu (nichts zu ändern).
- Sonst speichert sie die Eingaben in die Variablen von inBearbeiten. Dafür muss die Klasse Gericht entsprechende Setter definieren.
- In beiden Fällen macht die Funktion die Eingabefelder wieder leer, und die Variable inBearbeitung auch leer.

Problem: die Änderungen nach Bearbeitung werden nicht in der Benutzeroberfläche angezeigt. Dafür müsste das DefaultListModel eine Nachricht an die JList schicken, dass eines seiner Elemente sich geändert hat, und neu angezeigt werden muss - er bekommt aber selber diese Änderungen nicht mit. Da muss also etwas programmiert werden.
Die Klasse DefaultListModel verfügt über eine Methode fireContentsChanged​(...), die genau das Richtige tut. Aber sie ist protected traurig Sie müssen also die Klasse DefaultListModel erweitern, um darauf zuzugreifen.
Programmieren Sie eine Klasse GerichtListModel, die DefaultListModel<Gericht> erweitert und eine Methode gerichtGeaendert(Gericht g) enthält. Diese Methode sucht den Index des gegebenen Gerichtes (mit .indexOf()) und ruft fireContentsChanged(this,index,index) auf.
Benutzen Sie diese Klasse anstatt DefaultListModel in Ihrer Anwendung. In der speichern()-Funktion, rufen Sie ...gerichtGeaendert(inBearbeitung) auf. Jetzt sollte das Bearbeitungsformular funktionieren.

Fügen Sie ein zusätzliches JButton "Abbrechen" hinzu. Dieser macht die Eingabefelder und die Variable inBearbeitung wieder leer, und ändert den Speichern-Button so, dass er wieder "Hinzufügen" lautet.
