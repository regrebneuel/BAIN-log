---
title: "Lerneinheit zwei: Git, Teil 1/2 Funktion und Aufbau von Bibliothekssystemen"
author: "Gaby Leuenberger"
---
# Live from the road
Zur Lerneinheit zwei begrüsst uns der Dozent als digitaler Nomade aus dem Bus; erstaunlich, dass die mobile Datenverbindung auch ein Webex-Meeting erträgt! Bis auf zwei, drei kurze Aussetzer mit dem Ton verlief die Übertragung reibungslos. Ein grosses Hurra auf die moderne Kommunikationstechnologie!

Thematisch gab es zuallerst ein Feedback zur Umsetzung der Lerntagebücher, auch Rückfragen dazu wurden kurz besprochen, ohne aber gross in die Tiefe zu gehen. So wurde auf die Frage, ob man die Seite auch lokal ansehen kann, bevor man auf github pusht, auf das [github-Tutorial](https://pad.gwdg.de/12VJD7x4QgiRr498oLhnwg?view#GitHub-Pages-lokal) verwiesen. Und auf die Frage, wie man Bilder einbinden kann, kam zwar eine kurze Anleitung zum Gebrauch von Variablen in Jekyll, wo die base-URL der Seite angegeben wird, ich stelle mir aber vor, dass diese Erklärung für das Gros der Klasse zu abstrakt war. So wurde nicht erwähnt, dass die korrekte site.baseurl im `_config.yml` hinterlegt werden muss.

Ich für meinen Teil hatte ja etwas vor der ersten Lektion mein Tagebuch mit dem Theme "tale" eingerichtet, und dazu in meiner VM auch Ruby, Jekyll und benötigte dependencies installiert, für das lokale Anzeigen benötigt man aber &ndash; zumindest für mein Theme &ndash; andere `_config.yml`-Einstellungen, sodass wohl ein Eintrag des `_config.yml` in einer `.gitignore`-Datei nötig wäre, was ich bisher unterlassen habe (wenn ich die Anleitung richtig verstanden habe).

# Stichwort git
Womit wir nahtlos zum ersten Thema des heutigen Tages schreiten können: einer Kürzesteinführung zur Versionskontrollsoftware git.
Ich gebe zu, auch hier hörte nur mit einem halben Ohr zu, denn auch git gehört bei meiner Arbeit zum Alltag. Ohne eine solche Versionskontrolle wäre ein sinnvoller Umgang mit selbstentwickelter oder selbstgewarteter Software unmöglich.

Insofern war ich dann etwas erstaunt, dass als Anweisung zur Einarbeitung der Links zu den Lerntagebüchern nach `git fork pfad_zum_zu_forkenden_repository` kein eigener branch ausgecheckt werden sollte, wie das sonst üblich ist.[^1]

[^1]: Es widerspricht den Best Practices der Arbeit mit git, auf dem Master Branch zu arbeiten, damit Merge-Konflikte (das sind Konflikte beim Datenabgleich, die entstehen, wenn derselbe Code von verschiedenen Seiten unterschiedlich bearbeitet wurde) auf einem anderen Branch bereinigt werden können. Teils gilt gar, dass nie zwei auf demselben Branch arbeiten!

In unserem Fall spielte das aber nicht so eine grosse Rolle, weil ja klar war, dass alle einfach eine Zeile einfügen an derselben Stelle, womit die Merge-Konflikte leicht aufzulösen sind, indem die Zeilen einfach untereinandergereiht werden im Master.[^2]

[^2]: Man kann sich aber leicht vorstellen, wie unübersichtlich so ein Merge wird, wenn zehn Leute an unterschiedlichen Stellen im Code Sachen einfügen und löschen, oder gar die einen etwas ändern/verbessern, was andere einfach rausschmeissen und ganz anders lösen. Man kommt dann sehr schnell in Teufels Küche, weswegen es auch Workflows von git-Befehlen für Best Practices gibt. Immer zur Hand, wenn man nicht sicher ist, ist mit `git help everyday git` eine Übersicht über die am häufigsten verwendeten Kommandos je nach Benutzerszenario. Die einfachste, visuell ansprechendste Kurzanleitung zu git finde ich aber immer noch [diese hier](https://rogerdudler.github.io/git-guide/index.de.html) und für ausführliches Nachschlagen natürlich das [Git Community Book](http://book.git-scm.com/). Auch in der [library carpentry](https://librarycarpentry.org/lc-git/) gibt es einen git-Workshop, den muss ich mir aber erst mal noch ansehen.

Schliesslich schafften es fast alle, ihren eingefügten Link mit einem Push wieder auf github zu schicken und dort mit einem Pull-Request ihren Teil der Änderung abzuschliessen. Die Merge-Konflikte aufzulösen konnten wir Herrn Lohmeier überlassen.

# Metadaten
Die ganze Welt wird heute quasi von Metadaten gesteuert, und so ist das auch in Bibliotheken nicht anders. Einer der am weitesten verbreiteten Standards für Bibliotheksmetadaten ist Marc21 mit der Dateiendung `.mrc`, ein binäres Datenformat, das ohne spezielle Software nicht zu bearbeiten ist (eben mit einer Bibliothekssoftware oder einem spezifischen Editor wie [MarcEdit](https://marcedit.reeset.net)) und in MARCXML eine inhaltlich identische Repräsentation der Daten hat, die auch Menschenlesbar ist (https://www.dnb.de/DE/Professionell/Standardisierung/Standards/standards_node.html). Durch regional/international stark abweichende Katalogisierungsregeln ist es aber mit dem Standard nicht sehr weit her und die MARC-Daten unterscheiden sich dann stark und sind nicht ohne weiteres zwischen Bibliotheken austauschbar. Dem soll das neue Format [BIBFRAME]() in Zukunft Abhilfe schaffen, sowie zusätzlich Semantik (Linked Data) in die Daten bringen.
In einer Übung hatten wir dann noch die Gelegenheit, uns die Unterschiede von zwei Metadatenstandards vor Augen zu führen und zu analysieren. Ich tat dies direkt in im Terminal, in dem ich mir die Daten von sru.swissbib.ch mit `curl -o 'dateiname' "https://swissbib.ch/search/defaultdb?query=MYQUERYSETTINGS"`für Dublin Core und MARCXML in je eine Datei schrieb, sodass ich sie auch später nochmal in Ruhe anschauen kann.


# Koha
Schliesslich machten wir an den letzten Teil der Aufgabe für heute: Die Installation von Koha auf der virtuellen Linux-Maschine, die wir im [letzten Teil]({{site.baseurl}}/2020-09-10-TGL.md) in Betrieb genommen hatten.

Mit einer guten Installationsanleitung, wie wir sie auf der [BAIN-Lektionsseite](https://pad.gwdg.de/12VJD7x4QgiRr498oLhnwg?view#Funktion-und-Aufbau-von-Bibliothekssystemen-12) vor uns hatten, ist so eine Installationsroutine reine Copy/Paste-Arbeit.

Was mir auffiel, ist dass wir &ndash; in Abweichung von der [Anleitung](https://zefanjas.de/wie-man-koha-installiert-und-fuer-schulen-einrichtet-teil-1/) für eine "echte" Installation &ndash; auf diese Zeile verzichteten:

```
$ sudo mysql_secure_installation
```
Unsere Installation ist insofern "unsicher", als dass wir kein root-Passwort für die DB einrichten, was aber für unsere Zwecke nicht notwendig ist. (Dies würde im Installationsprozess noch weitere Prompts zur Folge haben, die mit "Y" quittiert werden können, und auch ein root-Passwort müsste festgelegt werden.)

Die Installation ging ganz glatt, bis ich am Web-Installer-Login hängenblieb, wo ich es vergeblich mit dem User *bibliothek* versuchte, bis uns gesagt wurde, dass der User eben *koha_bibliothek* heisst. Danach klickte ich mich durch, bis micht diese Fehlermeldung stoppte:
```
DBD::mysql::st execute failed: Cannot delete or update a parent row: a foreign key constraint fails at /usr/share/perl5/DBIx/RunSQL.pm line 279, <$args{…}> line 1. Something went wrong loading file /usr/share/koha/intranet/cgi-bin/installer/data/mysql/kohastructure.sql ([SQL ERROR]: – -- Table structure for table auth_types – DROP TABLE IF EXISTS auth_types ) at /usr/share/koha/lib/C4/Installer.pm line 557.
```
Da war dann aber auch die Stunde zu Ende und ich konnte das im Anschluss beheben. Was zuerst aussah, als hätte die Installation die Authentifizierungstabelle nicht sauber schreiben können, entpuppte sich wohl als Datenübertragungsproblem: ein Wiederaufrufen des Logins und anschliessendes Durchklicken ging fehlerfrei. Fazit: Man muss nicht alles verstehen!

Die Grundkonfiguration jedenfalls war mit der Anleitung ein Klacks:
![]({{site.baseurl}}/assets/success.png)

<hr>
