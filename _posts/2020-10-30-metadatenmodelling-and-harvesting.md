---
title: "Lerneinheit sechs: Metadaten modellieren und Schnittstellen nutzen (1/2)"
author: "Gaby Leuenberger"
date: 2020-10-30
---
# Im Herbst wird geharvested

Logisch eigentlich. Wir kommen heute unserem technischen Gesamtziel, aus den diversen Systemen die bibliothekarischen und archivarischen  Metadaten (MARC21, EAD und Dublin Core) zu sammeln, um sie dann einheitlich zu modellieren und für eine Suche zu indexieren, einen grossen Schritt näher: Wir werden mit VuFindHarvest &ndash; statt metha (das macht offenbar zu viele Probleme) &ndash; von den OAI-Schnittstellen von Koha, ArchivesSpace und DSpace Daten harvesten. Zur Erinnerung die grafische Übersicht der Datenflüsse, Tools und Schnittstellen, mit aktualisiertem Harvester:
![](https://pad.gwdg.de/uploads/upload_19a6e70e127583b4c50e24282bf7e3fd.png)  

Von Koha und ArchiveSpace werden wir die eingefügten Daten holen, die wir selbst eingefügt resp. von den Programmen aus über Schnittstellen geladen haben; für DSpace holen wir Daten aus einer Sample Community, weil jeden Samstag die Demo geputzt wird und die Daten von letztem Mal weg sind.

Solr ist das Webfrontend der Lucene-Software (beides von [Apache](https://lucene.apache.org/solr/)). Eine Alternative dazu wäre Elasticsearch (https://www.elastic.co/de/what-is/elasticsearch), eine verteilte Such- und Analytics-Maschine (die ebenfalls auf Lucene aufbaut), die unter anderem von Internetgiganten wie Uber, Ebay, vimeo und Netflix, aber auch von Industriekonzernen wie Airbus, Audi, Volvo verwendet wird (Quelle: [Elastic.co](https://www.elastic.co/de/customers/success-stories?usecase=enterprise-search)). Beide sind Open Source. Wer wissen möchte, wie so eine Suche genauer funktioniert und was weitere Use Cases für solche Software sind, kann [hier](https://www.elastic.co/de/what-is/elasticsearch) oder [hier](https://www.knowi.com/blog/what-is-elastic-search/) weiterlesen.

## Modellierung von Metadaten; Crosswalks mit Stolpersteinen

Die Schnittstellen, die wir bisher kennen lernten und die sich etabliert haben sind die Folgenden:
* Z39.50 (Library of Congress, Methusalem, braucht eine eigene Softwareschnittstelle)
* SRU, Search/Retrieve via URL (im Browser abrufbar)
* OAI-PMH - Open Archives Initiative Protocol for Metadata Harvesting (im Browser abrufbar)

Mit dem Wechsel von Aleph zu Alma sind wohl die letzten Tage der Uralt-Schnittstelle Z39.50 eingeläutet, auch wenn die Schnittstelle noch immer da ist. In der KnowledgeBase von ExLibris wird jedenfalls festgehalten, dass es pro Institution Zone bloss ein Profil dafür gibt und externe Suchen mit mehr als 10'000 Resultaten mit Fehler abbrechen ([ExLibris KnowledgeBase](https://knowledge.exlibrisgroup.com/Alma/Product_Documentation/010Alma_Online_Help_(English)/090Integrations_with_External_Systems/030Resource_Management/180Z39.50_Search)). Wir haben die im Kurs nur aus historischen Gründen kennen gelernt und verwenden sie nicht weiter.

Wir verwenden fürs Harvesting entweder SRU oder OAI-PMH, die über einfache Web-Requests für Bulk-Abfragen eingerichtet sind.

Dann gäbe es etwas neuer noch [Datacite](https://datacite.org/), ein Metadatenstandard eines internationalen Konsortiums, das vorwiegend für Forschungsdaten (DINI Zertifikat für Open Access Publikationen) verwendet wird; der ist sich erst am etablieren, den lassen wir mal aussen vor (mehr Infos dazu gibt's auf dieser [Wikipedia-Seite](https://de.wikipedia.org/wiki/DataCite)).


### Installation und Anwendung VuFindHarvest
Als erste Übung installieren wir VuFindHarvest (anstelle des ursprünglich vorgesehenen Tools [metha](https://github.com/miku/metha), das offenbar etwas zickig sein kann) und harvesten von den von uns aufgesetzten und angereicherten Quellen von Koha, ArchivesSpace und DSpace (da verwenden wir eine Demo-Source, weil unsere Einträge seit dem letzten Modul gelöscht wurden).  Das geht alles unproblematisch; der Harvester wird im vufindharvest-Verzeichnis mit diesem Befehl angeworfen:
```
php bin/harvest_oai.php --url=http://SOURCE --metadataPrefix=METADATASOURCE --set=SETNAME
```
Wobei in der Option `--url` SOURCE für die URL der Datenquelle steht, also die Webschnittstelle, wo die Daten geholt werden sollen, und in der Option `--metadataPrefix` das Format der abzuholenden Daten definiert wird. Die Option `--set` benötigten wir nur für DSpace, weil diese Metadaten noch mit Sets versehen sind. Man könnte auch, wenn man regelmässig "erntet", mit `--from=FROM`/`--until=UNTIL` einen Zeitraum festlegen, um nur die zuletzt dazugekommenen Records zu holen. Eine vollständige Übersicht über die Harvesting-Option bekommt man mit `php bin/harvest_oai.php -h`.

### Crosswalks statt Catwalks &ndash; MarcEdit als störrischer Esel, der dann aber doch noch gehorcht
Nun sind die Daten alle bei uns, aber jetzt müssen die noch zum Frisör, denn die sind ja in drei verschiedenen Metadaten-Formaten. Die Schere des Coiffeurs MarcEdit heisst XSLT (XSL Transformation, eine Sprache für die Transformation von XML-Dateien). Damit die Formate aufeinander passend gemacht werden können, muss man die jeweiligen Datenfelder aufeinander "mappen". Leider ist da meistens keine 1:1-Zuordnung möglich, weil den Daten teilweise sehr unterschiedliche Modellierungskonzepte zugrunde liegen. Im Idealfall wäre das aber verlustfrei möglich.

Für mich fangen hier die Probleme an. MarcEdit[^1] auf Ubuntu ist sehr störrischer Esel, und das GUI ist alles andere als benutzerfreundlich (sein Geburtsjahr ist dem GUI deutlich anzusehen). Zuerst müssen falsche Backslashes in den Pfaden der benötigten Funktionen geändert werden, weil sonst MarcEdit gar nicht funktioniert. So weit, so mühsam, aber gut.

![]({{site.baseurl}}/assets/marc_gui.png)
Das angestaubte MarcEdit-Gui.

[^1]: [MarcEdit](https://marcedit.reeset.net/) ist eine Freeware, die von dem Bibliothekar [Terry Reese](https://blog.reeset.net/about-me) 1999 in C# entwickelt wurde, ursprünglich für ein konkretes Datenbereinigungsprojekt an der Oregon State University. Heute arbeitet er an der Ohio State University.

Weiter also: wir versuchen, die Daten in Marc21xml zu überführen. Bei mir klappt das mit allen aussser den ArchivesSpace-Daten. MarcEdit meckert immer mit einem "File cannot be located. Error Number: -4". Und das, obwohl man doch über einen File Dialog den Pfad zur Datei übergibt? Ich steh vor einem Rätsel, dass ich in der Stunde auch nciht mehr gelöst bekomme.

Erst daheim, wo ich mir nochmal in Ruhe die Funktions-Einstellungen anschaue, dämmert mir, wo der Fehler liegt. Es ist für EAD=>MARC ein .xsl hinterlegt, dass es dort gar nicht gibt!! Es liegen dort mehrere verschiedene. Das EADtoMarc21slimXML.xsl funktionierte nicht, respektive produzierte kein Marcxml. Mit EADlitetoMARC21slimXML.xsl funktioniert es! Endlich kann ich mich der Vorbereitung der nächsten Lerneinheit widmen: OpenRefine!

Und zum Abschluss noch ein Tipp für Ubuntu-Nutzende, denen MarcEdit hängen bleibt oder nicht sauber aufstartet und sich nicht mehr schliessen lässt:

![]({{site.baseurl}}/assets/marc_kill.png)
How to kill MarcEdit.

Im Terminal mit `ps aux | grep marc` die MarcEdit-relevanten Prozesse auslesen; dann die Prozess-ID (im Bild markiert) kopieren und mit `kill -9` den Prozess abschiessen. Dann sollte das Fenster verschwinden und man kann neu anfangen.
