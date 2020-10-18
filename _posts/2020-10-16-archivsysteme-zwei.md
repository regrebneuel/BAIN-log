---
title: "Lerneinheit vier: ArchivesSpace 2/2"
author: "Gaby Leuenberger"
date: 2020-10-16
---
# Weiter geht's mit ArchivesSpace

Nun war unser Ansatz bei der Erfassung der ersten Einträge reichlich unorthodox, denn die Accession, mit der wir ja angefangen hatten, ist ja zu deutsch die Erwerbung. Die wird man wohl eher *nicht* von Anfang an auf public setzen, weil ja der ganze Bestand erst evaluiert werden muss (insbesondere auf Persönlichkeitsrechte und nötige Sperrfristen und Zugangsbeschränkungen). Aber die Accession dokumentiert halt den Erwerbungsprozess, darum fängt man damit an. Für unsere Zwecke war das Publi-Setzen aber ok, weil wir ja sowieso nur Bruchstücke erfassten.

Nach den üblichen Bemerkungen zu den Lerntagebüchern und Nachbemerkungen zur letzten Stunde zeigt uns Sebastian Meyer noch seine Archived Letters, um den tektonischen Aufbau nochmal zu verdeutlichen.

## Exkurs: Kleiner Ausflug zu AtoM zur Vorspeise
Um noch einen kleinen Oberflächen-Vergleich eines anderen OpenSource-Produkts zu bekommen, teilte uns ausserdem unsere Kollegin Karin ihre Arbeitsoberfläche, auf der sie mit [AtoM](https://www.accesstomemory.org/) (Access to Memory) für das Pestalozzianum Glasdias erschliesst. Ein interessanter Einblick, und offenbar ein deutlich offeneres Open-Source-Framework als ArchivesSpace, das seine Manuals nur zahlenden Mitgliedern zur Verfügung stellt ...

Doch zurück zu unseren Bemühungen:

Auf der nächsten Archive-Ebene steht in ArchivesSpace die Resource: Die ist grundsätzlich auf der obersten Ebene der ISAD(G)-Verzeichnungsstufen. Wenn es ein Einzelobjekt ist, das archiviert wird, kann auch das Einzelobjekt als Resource angelegt werden.

An diese Resource hängt man dann die eigentlichen Archival Objects dran. Also auf weiteren ISAD(G)-Verzeichnungsstufen (Bestand/Fonds, Serie/Series, Akte/File, Einzelstück/Item). Die können auf der Ebene Resource über "Add Child" hinzugefügt werden; aber wieder etwas tricky: Diese Struktur sieht man in der Resource immer erst, wenn man im EDIT-Modus ist!

1. Die Ebene Resource anwählen:
![](https://i.imgur.com/OqfLy7e.png
)
2. Die Resource bearbeiten:
![](https://i.imgur.com/FzWHhcY.png
)
3. Ein Kinddokument anhängen (oder Resource exportieren)
![](https://i.imgur.com/Dk1bInq.png
)

## Stichwort exportieren ...
Was uns gleich zu den ersten Übungen führt: Datenimport und -export. Zuerst den Import von EAD-Daten, den wir von der Seite [EADIVA](https://eadiva.com/2/sample-ead2002-files/) holen. In ArchivesSpace selbst liegt diese Funktion etwas versteckt hier im Background Job (tiefer geht der Screenshot mit [Greenshot](https://getgreenshot.org/) aus dem VDI heraus leider nicht, aber dahinter steht dann *Import Data*):
![](https://i.imgur.com/Z5JlUhq.png
)
Die Daten kommen hübsch an, wenn denn der Import funktioniert (Berliner Family Papers und Butterworth Paers brechen ab wegen offenbar unvollständiger XML-Daten, aber mit den *American Association of Industrial Editors Records* klappt es. Naja, beinahe. Der Vergleich des HTMLs mit dem ArchivesSpace Public Interface zeigt: Titel und Untertitel finden sich erst im Menü "Finding Aid", die Biographical History ist abgeschnitten, was wir aber auf die (kurze) Default-Länge des Description-Datenfeld der Demo-DB zurückführen. Klar ist: bloss weil etwas EAD ist, ist es noch längst nicht sorgenfrei importierbar...

Im Umkehrschluss versuchen wir, so eine Resource wieder zu exportieren als MARCXML; hier zeigt sich klar, was wir vermuteten: die unterschiedlichen Verzeichnistiefen von EAD, die für den Entstehungszusammenhang so wichtig sind, lassen sich logischerweise auf das Ressourcenzentrierte Marcxml nicht eins zu eins abbilden, sondern sie werden quasi flachgeklopft.

Was übrigens auch nicht funktioniert, obwohl der Import also completed angezeigt wird, ist ein Import des raw-XML von EADIVA mit der Import-Option als MARCXML. Da scheint die Mapping-Tabelle einfach nichts zu matchen, was einleuchtet, aber eigentlich trotzdem einen Fehler werfen sollte. Ah well... es bleibt bis auf Weiteres die ewige Herausforderung: uneingeschränkter Datenaustausch zwischen den Systemen: I'll be back!

Zum ArchivesSpace-Abschluss gibt es noch den Marktüberblick über die Archivsysteme. Ausser den schon erwähnten [AtoM](https://www.accesstomemory.org/) und eben ArchivesSpace, das vor allem in den USA über eine grosse Community verfügt, wird der Markt in der Schweiz vor allem von [scope.Archiv](http://www.scope.ch/) (zb. Staatsarchiv BS) und [~~CMISTAR~~ *heute:* CMI AIS](https://cmiag.ch/akten-management/archivierung/ais/) (ETH-Archiv) dominiert, wobei für die Online-Präsentation des digitalisierten Archivguts oft noch zusätzliche Software zur Anwendung kommt (z.B. [e-pics](https://www.e-pics.ethz.ch) und [e-manuscripta](https://www.e-manuscripta.ch/)).

## DSpace zum Dessert

Zum Abschluss des Tages schauen wir uns Repository-Software für Publikationen und Forschungsdaten an, konkret an einer [Onlinedemo für Version 6.x](https://demo.dspace.org/) von DSpace genauer an (weil es alle drei Bereiche abdecken kann). Mehr dazu aber ausführlich im nächsten Artikel.

Hier nur noch die grundsätzliche Unterscheidung von Open Access und Open Data:
Bei Open Access geht es um kostenfreien Zugang zu Forschungspublikationen, bei Open Data um die Forschungsdaten, die diesen Publikationen zugrundeliegen. Davon nochmal zu unterscheiden ist die Forschungsinformation, in denen es um Daten über Forschende und ihre Forschung (Welcher Bereich, Art der Finanzierung etc.) geht.

Entsprechend gibt es softwareseitig Repositorien für Open Access, Forschungsdatenspeicherung und Forschungsinformation (CRIS, Current Research Information Systems), wovon die bekanntesten sind:
-  [Zenodo](https://zenodo.org/) mit Publikationen und Forschungsdaten (vom CERN betrieben, umgesetzt mit Invenio; hat seinen Namen vom ersten Bibliothekar der antiken Bibliothek von Alexandria, Zenodotus); gilt als Best Practice
-  [DSpace](https://duraspace.org/dspace/) kann alle drei Bereiche verwalten (mit CRIS-Plugin). Beispiel der [Technischen Universität Hamburg](https://tore.tuhh.de/), wo es als Repositorium für Open-Access-Publikationen, Forschungsdaten und als Forschungsinformationssystem betrieben wird.
