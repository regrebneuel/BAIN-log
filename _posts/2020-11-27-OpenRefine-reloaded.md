---
title: "Lerneinheit acht: Suchmaschinen und Discovery-Systeme 1/2"
author: "Gaby Leuenberger"
date: 2020-11-27
---
# Langer Aufwasch vom letzten Mal, dann geht's auf Entdeckungsfahrt
In der heutigen Lektion rekapitulieren wir noch die letzte Anreicherungs- und Templatingübung, die die Möglichkeiten und Grenzen von OpenRefine recht schön aufzeigt. Ich habe dazu den Exkurs mit einem [Nachtrag ergänzt.]({{site.baseurl}}/2020-11-25/exkurs#nachtrag)

Anschliessend installieren und konfigurieren wir VuFind.

## OpenRefine, reloaded
Nachdem alle Templating-Fragen geklärt scheinen, machen wir uns daran, die XML-Daten zu validieren. Dies garantiert einem zumindest, dass die Daten auch dem hinterlegten XML-Schema entsprechen. Dabei können mit Regex auch relativ komplizierte Muster &ndash; bspw. für URLs, E-Mails oder ISBN &ndash; auf ihre formale Richtigkeit überprüft werden.

Wenn man aber bei der Reconciliation falsche Daten zuweist, nützt die Validierung dann auch nix. Da gilt klar das gigo-Prinzip: garbage in &ndash; garbage out!

Prüfen kann man das unter Linux mit dem eingebauten Tool `xmllint`:
```bash
xmllint PATH/TO/FILENAME.xml --noout --schema PATH/TO/SCHEMA.xsd
```
Die Eingabe überrascht uns mit einer Reihe Fehler: Wir haben doch glatt vergessen, ein Subfield-Element im Template zu schliessen.
![]({{site.baseurl}}/assets/VuFind/xmllint_error.png)
Also nochmal von vorne, und dann klappt das auch:
![]({{site.baseurl}}/assets/VuFind/xmllint.png)

### XML-Deklarationen
Dazu gibt's auch noch einen kurzen Exkurs zu den XML-Deklarationen, die wir in unserem Template erstmal einfach weggelassen haben. Sie stehen immer am Anfang einer XML-Datei, das XML kann aber auch ohne validiert werden.
Die Deklaration sagt dem Parser, was für eine Datei jetzt kommt (analog zu HTML-Dateien, die auch eine Deklaration am Anfang haben). Die einzige Pflichtangabe dabei ist `version`, aber `encoding` gehört in der Praxis eigentlich auch immer mit. Das optionale Attribut `standalone` mit Wert "yes" oder "no" zeigt an, ob eine DTD, also eine Document Type Definition enthalten (yes) ist oder referenziert wird (no); falls man es weglässt, setzt der Parser das Attribut selbständig (Default ist yes, ausser, es wird explizit eine DTD referenziert).

### Vergleich mit alternativen Tools zu OpenRefine
Als Feedback zu meinem Lerntagebuch-Eintrag werden noch alternative Tools vorgestellt, die anstelle von OpenRefine Anwendung finden. Je nachdem, welche Bearbeitungen man vornimmt und welche anderweitigen Kenntnisse man mitbringt, gibt es Gründe, auch andere Tools zu verwenden, beispielsweise:
- [Catmandu](https://librecat.org/) (Perl)
- [Metafacture](https://github.com/metafacture/metafacture-core) (Java)
- MarcEdit (für MARC21, kennen wir ja schon, als Variante evtl. noch [pymarc](https://pymarc.readthedocs.io/))

Speziell an den ersten beiden Tools ist die eigene Metasprache Fix, die ähnlich wie GREL auch ohne spezifische Perl- oder Java-Kenntisse die Bearbeitung der Daten ermöglichen (ursprünglich aus Catmandu, neu implementiert auch für Metafacture). Schaut man die Tools auf OpenHub an, sind alles eher Orchideen: eher kleine, aber wohl recht enthusiastische Contributor-Basis. Catmandu wurde in unserem Team mal angeschaut; bevor ich dort anfing zu arbeiten, wurde aber der Umstieg auf Python beschlossen wurde, womit sich das dann wohl auch erübrigt (Mental Note: pymarc anschauen).

### JSON-Schnittstellen
Dann wurden noch ein paar Worte zu JSON-API-Schnittstellen verloren, die heute State-of-the-art sind deren Antworten dann eben in JSON zurückkommen statt XML, was moderne Browser auch ziemlich hübsch anzeigen können ([libd-gnd](https://lobid.org/gnd/api), die wir mit OpenRefine in der [letzten Übung]({{site.baseurl}}/2020-11-25/exkurs) angezapft hatten, gehört da auch dazu).

[scrAPIr](https://scrapir.org/), das andere Beispiel, sieht aus wie ein Tool, das einen niederschwelligen Zugang zu Api-Schnittstellen bietet, um zu lernen wie man web scraping betreiben kann. So kann man damit die APIs von grossen Webseiten durchsuchen: hier kann man beispielsweise [auf Stackexchange die letzten 100 Fragen zu Python Data Frames suchen.](https://scrapir.org/data-management?api=Stack_Overflow_Search)

### Wo ein LIDO ist, gibt's noch lange keinen Strand
Zu guter Letzt gibt uns dann Sebastian Meyer noch einen rasanten Schnellaufwasch zum Metadatenstandard LIDO (Lightweight Information Describing Objects), der zur Beschreibung von Kulturobjekten vorwiegend von Museen und Kunsthäusern verwendet wird. Da hatte ich mir eigentlich erhofft, evtl. etwas zu lernen, was mich bei meiner Arbeit damit weiterbringen könnte.

Leider erfahre ich da nicht wirklich etwas Neues, aber ich werde immerhin in meiner Erfahrung bestärkt, dass ich da keinen leichten Stand haben werde, weil es zu meinen Aufgaben gehört, an Crosswalks aus LIDO in andere Formate mitzuarbeiten.

Hellhörig war ich aber zuvor beim Hinweis auf einen [bavaricon-Vortrag](https://www.edvtage.de/magic/show_image.php?id=310350&download=1) im Feedback auf [Giulias Lerntagebuch](https://damicogiulia.github.io/BAIN-Blog/2020/11/25/tag7.html) geworden. Doch auch dort wurde im Endeffekt am Schluss die Datenbank reverse-engineered, um einen eigenen Export zu bauen :see-no-evil:

## Endlich VuFind
Die Aufarbeitung und die Exkurse hatten einige Zeit gekostet; es war aber spannend und lehrreich.

Die Installation von VuFind wurde dann von den Dozenten im Playalong-Stil vorexerziert, wohl weil sie in ihrem Testlauf einigen Fallstricken begegnet waren, die sie uns ersparen wollten:

Es wurde noch einmal erwähnt, weswegen üblicherweise pro Software ein Server verwendet wird: Dependency hell lässt grüssen. Zu gross ist die Gefahr, dass sich verschiedene Systeme nicht vertragen oder eben gar sich ausschliessende Abhängigkeiten haben. Durch Containerisierung (Stichworte: [Docker/Podman, Kubernetes, OpenShift](https://opensource.com/article/18/8/sysadmins-guide-containers)[^1]) können heute diese Probleme umgangen werden und viele Ressource gespart werden, aber das ist eine andere Geschichte.

[^1:] Unterschied Podman und Docker: https://www.netways.de/blog/2019/05/31/podman-ist-dem-docker-sein-tod/; [Openshift](https://de.wikipedia.org/wiki/OpenShift)

Auf unserem Ubuntu 20.04 vertragen sich interessantweise alle von uns verwendeten Systeme, allerdings stellt sich bei VuFind ein Hindernis in den Weg, das bei [ArchivesSpace]({{site.baseurl}}/2020-10-09/archivsysteme) Melanie und mich ausgebremst hatte: Dieses Tool kann (noch) nicht mit der neusten Java-Installation, weswegen dann Solr nicht starten kann.

Ohne Anleitung durch unsere Dozenten müssten wir uns an dieser [Anleitung für Ubuntu]( https://vufind.org/wiki/installation:ubuntu) orientieren. Daraus haben sie aber die wichtigsten Verzweigungen und Fallstricke [festgehalten](https://pad.gwdg.de/ywogyRNTQ_CTg9PvrQywsQ?both#Installation-und-Konfiguration-von-VuFind).

Dabei kommt das eigentlich für flüssig vorbereitetes Abarbeiten vorbereitete Skript ins Stocken, weil diese in einem Block angegebenen Shell-Befehle einzeln hätten ausgeführt werden sollen:
```bash
sudo apt install -y openjdk-8-jdk
sudo update-alternatives --set java /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java
sudo rm /usr/lib/jvm/default-java
sudo ln -s /usr/lib/jvm/java-8-openjdk-amd64 /usr/lib/jvm/default-java
```

Aber ich greife vor: **zuerst** galt es, über `dpkg`, eine Alternative zu `apt` oder `apt-get`, die VuFind-Debian-Pakete zu installieren. Doch auch das wirft einen Fehler:

![]({{site.baseurl}}/assets/VuFind/vufind_install.png)

Also zuerst noch mit `sudo apt-get update` die Paketquellen aktualisieren und (optional `sudo apt-get upgrade` gleich updaten), sowie `sudo apt --fix-broken install` die fehlenden Pakete nachinstallieren:

![]({{site.baseurl}}/assets/VuFind/fixbrokeninstall.png)

Dann die Datenbank (auf Ubuntu 20.04 MariaDB, äquivalent zu MySql) mit `sudo /usr/bin/mysql_secure_installation` aufsetzen (enter, root pwd setzen, durch den Rest durchklicken mit enter) und einrichten:

![]({{site.baseurl}}/assets/VuFind/mysqul-setup.png)

und entgegen der Anleitung mit `chown` von einigen Dateien die Rechte anpassen, also den Owner changen:

![]({{site.baseurl}}/assets/VuFind/changeowner.png)

Danach, wenn die richtige Java-Version aktiv ist, startet Solr ohne Mucken:

![]({{site.baseurl}}/assets/VuFind/solr.png)

Auch VuFind läuft im Browser und muss noch gemäss [Anleitung](https://pad.gwdg.de/ywogyRNTQ_CTg9PvrQywsQ?both#Configuring-and-starting-VuFind--Auto-Configuration) fertig konfiguriert werden, und mit dem Laden von ersten Testimporten ist auch dieses Modul bereits zu Ende.

```bash
/usr/local/vufind/import-marc.sh /usr/local/vufind/tests/data/journals.mrc
/usr/local/vufind/import-marc.sh /usr/local/vufind/tests/data/geo.mrc
/usr/local/vufind/import-marc.sh /usr/local/vufind/tests/data/authoritybibs.mrc
```
