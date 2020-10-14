---
title: "Lerneinheit vier: ArchivesSpace 1/2"
author: "Gaby Leuenberger"
date: 2020-10-09
---
# Ein Weg mit Hindernissen ...

Zum Einstieg in den Tag musste ich tief in meinen Erst- und Zweitsemester-Erinnerungen graben. Archive sind so meine Stärke nicht, doch was wir zum Ankurbeln der Erinnerungen erzählt bekamen, war mir dann doch nicht ganz unvertraut:

Beispielsweise, das zentrale Unterschied zu Bibliotheken die folgenden sind:
* Ziel ist die Bewahrung und die Erwerbung folgt meist einer Routine
* Erschlossen wird mit Findmitteln, nicht mit einem Katalog.
* Provenienz vor Ressourcen; es ist wichtig, wer der Aktenbildner ist und wie der Entstehungszusammengang der Dokumente ist
* Die Objekte sind in der Regel Unikate
* Kontext, insbesondere auch Entstehungskontext ist wichtig;
* Archive sind Zugangsbeschränkt, vor allem wegen Persönlichkeitsrechte oder Staatsgeheimnissen

## ISAD(G)eht aus dem Leim...

Auch beim Standard ISAD(G) klingeln meine Ohren, aber in allzu guter Erinnerung hab ich das Fach trotzdem nicht. Die Abkürzung steht für International Standard Archival Description (General) und ist ein Anwendungsstandard zur Verzeichnung Archivischer Unterlagen. [^1] Der STandard bildet mit mehrstufiger Verzeichnung im Provenienzprinzip den Entstehungszusammenhang ab.

[^1]: Quelle: https://de.wikipedia.org/wiki/ISAD(G); die umfangreiche Schweizerische Richtlinie für die Umsetzung von ISAD(G) gibt es [hier](https://vsa-aas.ch/wp-content/uploads/2015/06/Richtlinien_ISAD_G_VSA_d.pdf).

Der Standard ist etwas in die Jahre gekommen und hat Defizite:
- ein einzelner Datensatz ist nur im Kontext verständlich ("Protokoll"); Lösungsansatz: RIC (Records in Contexts, mit Linked-Data-Ansatz)
- eindimensionale Tektonik: man kann ein Ojekt nur an EINEM Ort anhängen, und das ist abhängig von der Erschliessung des Archivars, also ein Stück weit arbiträr.
- es gibt keine Vorgaben für die digitale Langzeitarchivierung (zu alter Standard)

Die erste Übung dann offenbarte mir, was mich immer so abschreckte: die unübersichtliche Verschachtelung der Dossiers in den Dateibäumen will mir einfach nicht schmecken.

## EAD und RiC to the rescue!

Vielversprechender klingt da der Ansatz, [mit Records in Context](https://www.ica.org/standards/RiC/RiC-O_v0-1.html) nach Linked-Data-Prinzipien Beziehungen zwischen den Entitäten zu ermöglichen, und so die eindimensionale Tektonik aufzubrechen und sogar Archive zueinander in Beziehung zu stellen. Die Ontologie dazu gibt es für vom SESY-Modul angefixte als RDF [hier](https://www.ica.org/standards/RiC/RiC-O_v0-1.rdf).

<!---
Übung eins:
## Antworten auf die Fragen

Gruppe 1 (Zürich):

1. Trefferliste: Titel, Zeitraum, Signatur, BS zusätzlich Stufe,  Archivplan & Benützbarkeit
2. Verzeichnungsstufen: wird sowohl bie BS als auch bei ETH direkt in Suchergebnissen angezeigt. Bei ETH kann zusätzlich nach Verzeichnungsstufe gefiltert werden (Bestand, Serie, Dossier, Einzelstück...)
3. Informationsbereiche: in ETH nicht sofort erkennbar, Basel schon
4. Unterschiede: Basel ist übersichtlicher. Archivplansuche bei ETH etwas versteckt. Gibt es bei ETH eine Feldsuche? Umfangreiche Suchmöglichkeiten in Basel, bei ETH nicht ganz klar, ein Suchschlitz mit google-Prinzip, Bs auf Expertensuche ausgelegt
5. Vergleich Bibkatalog: Nicht auf Kontext, Fokus auf Exemplare
6. Sonstiges: ETH hat viel mehr resultate
-->
Um Archive für die digitale Zukunft zu rüsten, wurde auch ein XML-Standard entwickelt: EAD oder [Encoded Archival Description](https://de.wikipedia.org/wiki/Encoded_Archival_Description); ein Standard zur Beschreibung von Findmitteln. Archive verlinken ihre Online-Findmittel mit Linked Data zu Wikidata über die property:archivesAt auf [Wikidata](https://www.wikidata.org/wiki/Wikidata:Main_Page).

## To infinity and ...

Weiter zur zweiten Übung, wo es galt, das zuhause ohne Probleme installierte ArchivesSpace zu konfigurieren. Interessanterweise kommen sich Koha und ArchivesSpace nicht in die Quere. Es ist eigentlich gar nicht State-of-the-Art, zwei Systeme auf demselben Server laufen zu lassen, weil es immer wieder zu Konflikten von Abhängigkeiten kommen kann.

Typischerweise macht man das heute entweder mit virtualisierten Servern oder in abgekapselten Containern, die auf einem Server laufen.

Aber los jetzt, es gilt, in ArchiveSpace eigene Datensätze zu erstellen. Intuitiv sollen wir das machen, und nicht einfach der [Trainingsanleitung](https://guides.nyu.edu/ld.php?content_id=23198351) folgen, aber ich hätte mich damit bestimmt leichter getan. Die gefühlt tausend Felder, die es zu befüllen gibt, liegen mir quer im Kopf und ich kann wenig mit den Begriffen anfangen. Trotzdem hangle ich mich von Accession zu Record und will das auf http:localhost:8081 im public space anzeigen und ...

## beyond: 500
![](https://pad.gwdg.de/uploads/upload_58179a0d9e4fe84293c37a9f5fa1479f.png)
Kein Anschluss unter dieser Nummer. 😧

Ich hatte zuerst befürchtet, es hätte was mit der tmux Session zu tun, in der ich ArchiveSpace mit `tmux new -s archivespace` gestartet hatte.[^2]
![]({{site.baseurl}}/assets/archivespace_tmux.png)
Eine tmux-Session.

[^2]: ArchiveSpace läuft immer in einer Shell, wenn man die Konsole schliesst, beendet das automatisch auch die Applikation. `tmux` löst `screen` ab, ein [Terminal Multiplexer](https://en.wikipedia.org/wiki/GNU_Screen), der wegen Sicherheitslücken z.b. von RHEL 8 gestrichen wurde, wie ich bei der Arbeit gelernt hatte. Das wollte ich doch gleich anwenden, und ArchiveSpace im Hintergrund laufen lassen, was auch tadellos funktioniert, wenn ...

Das war also nix. Fehlersuche war angesagt, aber wie? Ein Server Error 500 ist so ungefähr die generischste Fehlermeldung, die einem ein Server vor den Latz knallen kann. Und im Hintergrund rödelte ArchiveSpace immer weiter. Alle Schnittstellten funktionierten, bloss auf Port 8081 kam keine Antwort.

Ich stoppte die Applikation und schaute mir den Output mal durch nach einem Fehler, was aber bei dem Ausstoss an Text Fischen im Trben gleicht. Also bin ich die Log Files suchen gegangen, und habe sie auch gefunden unter ~./archivspace/logs/archivesspace.out Dort liegt natürlich dieselbe Buchstabenwüste wie in der Konsole, also mit `grep -C 100 'WARNING: ERROR:' archivesspace.out` die Ausgabe eingegrenzt und unseren Dozenten zur Rettung geschickt.

Dies führte tatsächlich zum Ziel! Ich hatte ja den erstbesten Fehler im Verdacht; da meckerte Ruby, dass ein Gem fehle. Tatsächlich aber waren bei mir und Melanie als einzige eine zu neue Java-Version installiert, mit der das für das Public Interface von ArchiveSpace zuständige Modul (noch) nicht umgehen kann.
![]({{site.baseurl}}/assets/jdk.png)
Der Screenshot der Rettung

Kurz vor der schon bald folgenden nächsten Sitzung waren unsere Rechner also auch wieder voll einsatzfähig, YEAY!!
