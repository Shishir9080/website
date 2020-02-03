---
layout: page_noise3d
title: 3D input data voor geluidssimulaties versie 0.2 (experimenteel), opgeleverd in voorjaar 2019
permalink: /opendata/noise3d/nl
is_dutch: true
map: true
---

![](/img/projects/noise3d_banner.jpg)

<div class="well"><b>Feedback Sessie op 6 februari 2020</b><br/><br/>
	Op donderdag 6 februari 2020 organiseren we een feedback sessie over versie 0.3 (die momenteel in ontwikkeling is) bij het Kadaster in Rotterdam (10:00-12:00). Tijdens deze sessie zullen we uitleg geven over de stand van zaken van onze methode en ontvangen we graag uw feedback voor onze verdere ontwikkelingen.
	U kunt zich <a href="https://docs.google.com/forms/d/e/1FAIpQLSdlVlcyZ-vCFcH5KYUKeSWgd7MX7t0msp4dL3wnKpD0fiHAPg/viewform">hier</a> aanmelden voor deze sessie. U kunt ook feedback geven op de data via het feedback formulier <a href="https://docs.google.com/forms/d/e/1FAIpQLSfgWxv-5xdSWcEAxmmu6tnzwlc9fw6N-wHQuJLnnSNJv2NCtg/viewform?usp=sf_link">hier</a> 
	</div>

- - -

* Table of Content
{:toc}

- - -
## Wat is 3D geluid NL? Introductie
De afgelopen 3 jaar hebben Kadaster, RWS, TU Delft, RIVM en IPO samengewerkt aan de automatische reconstructie van 3D input data voor geluidssimulaties. Hierbij wordt gebruik gemaakt van landsdekkende gegevensbronnen zoals de BGT, de BAG en het AHN en wordt alle modelinformatie van de fysieke ruimte gegenereerd die nodig is voor het uitvoeren van geluidssimulaties. Het bevat een beschrijving van het hoogteverloop van het terrein, de eigenschappen van het bodemoppervlak en de geometrie van gebouwen.

Meer uitleg over ons project dat startte in 2017, is [hier]({{ "/projects/noise3d/" | prepend: site.baseurl  }}) te vinden.


## Beschrijving test data versie 0.3
Onze methode heeft als doel om zo veel mogelijk detail en nauwkeurigheid te behouden, en tegelijkertijd de data klein te houden en deze te laten aansluiten om de huidige beschikbare geluidsimulatie software systemen. 

Met versie 0.3 bieden we 3 input lagen aan voor geluid studies. Namelijk:
1. Gebouwen
2. Bodemvlakken met geluidreflectie- en absorptie waarden
3. Hoogte-beschrijving van het terrein

Deze 3 lagen zijn volledig automatisch gegenereerd op basis van BAG, BGT en AHN. De wijze waarop we dit hebben gedaan, is hieronder in meer detail beschreven. 

De belangrijkste veranderingen ten opzichte van versie 0.2 zijn:
* Sterk verbeterde LoD 1.3 gebouw modellen
* Terrein opgeleverd als TIN in plaats van hoogtelijnen

Voor deze test data zijn voorlopige keuzes gemaakt ten aanzien van vereenvoudiging van geometrieën in de basisgegevens, hoogte-differentiatie tussen aansluitende dakdelen, minimale afmetingen, etc. Aan de hand van uw reacties kunnen deze instellingen in een volgende versie nog worden aangepast.

Voordat de 3D input data wordt opgeschaald tot landsdekkend niveau wordt het product voor een testgebied ter beschikking gesteld aan potentiele eindgebruikers, met als doel om feedback te verzamelen. Het testgebied beslaat een deel van het Rijnmondgebied, om precies te zijn: de kaartbladen 37ez2, 37fz1, 37gn2 en 37hn1, waarbinnen een deel van de gemeentes Schiedam en Rotterdam vallen.

![Sample area v0.2]({{ "testarea_v02_extent.png" | prepend: site.baseurl }})

Deze gegevens kunnen direct als input worden gebruikt in software die op basis van Standaard Rekenmethode II van het RMG2012 (SRM2) rekent, zoals GeoMilieu en WinHavik.


### Gebouwen

<iframe src="{{ "lod13map.html" | prepend: site.baseurl }}" id="demo" class="w-full" allowfullscreen="" mozallowfullscreen="true" webkitallowfullscreen="true" style="height:400px; width:100%; border:none;"></iframe>

Voor de modellering van de gebouwen is gebruik gemaakt van BAG panden. De toekenning van gebouwhoogtes gebeurt aan de hand van de AHN-puntenwolk. Hiermee wordt de 2D informatie van de BAG-panden omgezet tot 3D blokvormen. 

Het modelleren van elk BAG pand met slechts een enkele hoogte (LoD 1.0) kan tot fouten leiden als een pand in werkelijkheid verschillende hoogtes heeft bijvoorbeeld in het geval van een kerk met toren of een huis met aanliggende garage. Daarom hebben we ook gekeken hoe we BAG panden met hoogtesprongen kunnen modelleren.
Dit is de zogenaamde LoD1.3 representatie, dat wil zeggen dat er binnen ieder BAG-pand onderscheid gemaakt wordt tussen dakdelen als relevante hoogteverschillen tussen die dakdelen daar aanleiding toe geven.
In versie 0.3 is een hoogtesprong relevant vanaf 3 meter, grofweg 1 verdieping. 

Naast de LoD 1.3 gebouwen zijn ook LoD 1.0 gebouwen beschikbaar. Hierin heeft ieder BAG pand slechts een enkele hoogte waarde, en vind er dus geen opsplitsing plaats op basis van hoogtesprongen.

De hoogte van de dakdelen is uitgedrukt in het 75-percentiel van alle AHN-punten die binnen dat dakdeel vallen. Als extra informatie wordt in de LoD 1 representatie ook het 95-percentiel van de hoogte van de puntenwolk meegeleverd.

Op de gebouwen zijn de volgende opmerkingen van toepassing:
1. Complexe gebouwen. Het LoD 1.3 reconstructie process werkt op basis van daklijnen die in de puntenwolk worden gedecteerd. Als het aantal lijnen hoog is, neemt de verwerkingstijd van de LoD 1.3 reconstructie sterk toe. Gebouwen met een hoog aantal gedetecteerde lijnen (meer dan 400) zijn daarom als 'complex' aangemerkt met het `is_complex` attribuut. Bij deze complexe gebouwen zijn niet alle gedetecteerde lijnen meegenonen in de reconstructie waardoor de modellerings fout in theorie groter kan zijn.
2. Naast de BAG panden zijn in deze versie ook de overige bouwwerken met het type 'open loods' uit de BGT meegenomen. Deze objecten missen een aantal attributen.
3. In een voorwerkingstap is gepoogd ondergrondse panden (zoals ondergrondse parkeergarages) uit de BAG te filteren. Door onvolkomendheden in dit process zijn echter ook een aantal bovengrondse gebouwen uitgefilterd (voorbeeld: de Martkhal in Rotterdam).
4. Als er geen hoogtepunten en/of dakvlakken voor een gebouw zijn gedetecteerd is de dakhoogte op `0` gezet. Zie ook het `dak_type` attribuut.
5. De LoD 1.0 gebouwen zijn identiek aan versie 0.2.


![Sample area v0.2]({{ "building_lod.png" | prepend: site.baseurl }})

<!-- Meer uitleg over de reconstructie van LoD1.3 staat uitgelegd in de volgende slides.

<iframe src="https://docs.google.com/presentation/d/e/2PACX-1vTooIsoh8wN8nbd_xv4YOgo0blfdm7dSG4NSpIvgL5meQ4yz4YiL1n3TGjvdpJea20x1e6r-E0woeDc/embed?start=false&loop=false&delayms=3000" frameborder="0" width="480" height="299" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe> -->

### Hoogtebeschrijving Terrein
Voor de modellering van het hoogteverloop van de bodem vormt de BGT de basis. De grenslijnen tussen verschillende objecttypen in de BGT worden hiertoe verrijkt met hoogte-gegevens uit het AHN. Waar dit resulteert in overbodige hoogtelijnen, omdat deze geen wezenlijke verandering in het hoogteverloop teweeg brengen, zijn deze verwijderd om de tijd voor dataverwerking en geluidberekeningen te beperken. Ook kan het voorkomen dat er wel sprake is van hoogteverloop zonder dat er een overgang tussen BGT-objecttypen is. Voor die situaties is een TIN geconstrueerd van het hoogteverschil tussen het AHN en de met hoogte verrijkte BGT-data. Daar waar hoogteverschillen van meer dan 0,5m met het AHN zouden optreden zijn aanvullende hoogtelijnen gegenereerd en voorzien van de AHN hoogte.
Op locaties waar er ongelijkvloerse kruisingen zijn in de infrastructuur, die in de BGT zijn geadministreerd als brugdek, is de AHN-puntenwolk buiten beschouwing gelaten omdat die op deze locaties niet de hoogte-informatie van het maaiveld, maar van het brugdek weergeeft.

### Bodemgebieden
Ook voor de modellering van akoestisch reflecterende en akoestisch absorberende oppervlakten wordt gebruik gemaakt van de geometrie en thematische informatie in de BGT. Bodemgebieden kennen geen hoogte-informatie (die wordt via de hoogtelijnen in de geluidberekeningen verwerkt). Aansluitende bodemgebieden met dezelfde akoestische eigenschappen worden samengevoegd. Vervolgens wordt de geometrie vereenvoudigd door kleine oppervlakten (6 m2) met eigenschappen die afwijken van de aangrenzende vlakken buiten beschouwing te laten en vormpunten te verwijderen die tot onnodige detaillering zouden leiden. Hierbij is een tolerantie van 15 cm in de ligging van een lijn aangehouden.

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;border:none;}
.tg td{padding:10px 5px;border-style:solid;border-width:0px;overflow:hidden;word-break:normal;}
.tg th{font-weight:normal;padding:10px 5px;border-style:solid;border-width:0px;overflow:hidden;word-break:normal;}
.tg .tg-fymr{font-weight:bold;border-color:inherit;text-align:left;vertical-align:top}
.tg .tg-pcvp{border-color:inherit;text-align:left;vertical-align:top;background-color: #ecf0f1;}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-fymr">BGT klasse en <code>fysiekVoorkomen</code> attribuutwaarde</th>
    <th class="tg-fymr">Akoestische classificatie</th>
  </tr> 
  <tr>
    <td class="tg-pcvp">OndersteunendWaterdeel (alles)</td>
    <td class="tg-pcvp">absorberend</td>
  </tr>
  <tr>
    <td class="tg-0pky">OnbegroeidTerreindeel (erf, gesloten verharding, open verharding, half verhard</td>
    <td class="tg-0pky">reflecterend</td>
  </tr>
  <tr>
    <td class="tg-pcvp">OnbegroeidTerreindeel (onverhard, zand)</td>
    <td class="tg-pcvp">absorberend</td>
  </tr>
  <tr>
    <td class="tg-0pky">BegroeidTerreindeel</td>
    <td class="tg-0pky">absorberend</td>
  </tr>
  <tr>
    <td class="tg-pcvp">Pand (alles)</td>
    <td class="tg-pcvp">reflecterend</td>
  </tr>
  <tr>
    <td class="tg-0pky">Scheiding (alles)</td>
    <td class="tg-0pky">reflecterend</td>
  </tr>
  <tr>
    <td class="tg-pcvp">Kunstwerkdeel (alles)</td>
    <td class="tg-pcvp">reflecterend</td>
  </tr>
  <tr>
    <td class="tg-0pky">OverigBouwwerk (alles)</td>
    <td class="tg-0pky">reflecterend</td>
  </tr>
  <tr>
    <td class="tg-pcvp">Overbruggingsdeel (alles)</td>
    <td class="tg-pcvp">reflecterend</td>
  </tr>
  <tr>
    <td class="tg-0pky">Wegdeel (anders dan ruiterpad en onverhard)</td>
    <td class="tg-0pky">reflecterend</td>
  </tr>
  <tr>
    <td class="tg-pcvp">Wegdeel (ruiterpad, onverhard)</td>
    <td class="tg-pcvp">absorberend</td>
  </tr>
  <tr>
    <td class="tg-0pky">OndersteunendWegdeel (verkeerseiland, gesloten verharding, open verharding, half verhard)</td>
    <td class="tg-0pky">reflecterend</td>
  </tr>
  <tr>
    <td class="tg-pcvp">OndersteunendWegdeel (berm, onverhard, groenvoorziening)</td>
    <td class="tg-pcvp">absorberend</td>
  </tr>
  <tr>
    <td class="tg-0pky">Waterdeel (alles)</td>
    <td class="tg-0pky">reflecterend</td>
  </tr>
</table>

## Downloads

Dit testbestand is beschikbaar in *ESRI shape*.

De gebruikte bronbestanden zijn:
* BGT: datum 11-02-2019, <a href="{{ "source_bgt.zip" | prepend: "/download/noise3d/v02/" | prepend: site.baseurl }}">[download bronbestanden]</a>
* BAG: datum 25-02-2019, <a href="{{ "source_bag.zip" | prepend: "/download/noise3d/v02/" | prepend: site.baseurl }}">[download bronbestanden]</a>
* AHN: versie 3, download bronbestanden van PDOK: [37ez2](https://geodata.nationaalgeoregister.nl/ahn3/extract/ahn3_laz/C_37EZ2.LAZ), [37fz1](https://geodata.nationaalgeoregister.nl/ahn3/extract/ahn3_laz/C_37FZ1.LAZ), [37gn2](https://geodata.nationaalgeoregister.nl/ahn3/extract/ahn3_laz/C_37GN2.LAZ), [37hn1](https://geodata.nationaalgeoregister.nl/ahn3/extract/ahn3_laz/C_37HN1.LAZ)

Onderstaand het download overzicht.

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;border:none;}
.tg td{padding:10px 5px;border-style:solid;border-width:0px;overflow:hidden;word-break:normal;}
.tg th{font-weight:normal;padding:10px 5px;border-style:solid;border-width:0px;overflow:hidden;word-break:normal;}
.tg .tg-fymr{font-weight:bold;border-color:inherit;text-align:left;vertical-align:top}
.tg .tg-pcvp{border-color:inherit;text-align:left;vertical-align:top;background-color: #ecf0f1;}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-fymr">Klasse</th>
    <th class="tg-fymr">Uitleg</th>
    <th class="tg-fymr">Bestandsnaam</th>
    <th class="tg-fymr">Download</th>
  </tr>
  <tr>
    <td class="tg-pcvp">Bodemvlakken</td>
    <td class="tg-pcvp">Bodemvlakken geklassificeerd naar bodemreflectie eigenschap. Objecten kleiner dan 6 vierkante meter zijn aan hun buren toegevoegd.</td>
    <td class="tg-pcvp">&lt;tile id&gt;_bodemvlakken</td>
    <td class="tg-pcvp">
      <a href="{{ "bodemvlakken.zip" | prepend: "/download/noise3d/v02/" | prepend: site.baseurl }}">[ESRI Shapefile]</a><br/>
    </td>
  </tr>
  <tr>
    <td class="tg-0pky">Hoogtelijnen<br></td>
    <td class="tg-0pky">Hoogtebeschrijving van het terrein beschreven met 3D lijnen (dus geen iso lijnen of breeklijnen)</td>
    <td class="tg-0pky">&lt;tile id&gt;_hoogtelijnen</td>
    <td class="tg-0pky">
      <a href="{{ "hoogtelijnen.zip" | prepend: "/download/noise3d/v02/" | prepend: site.baseurl }}">[ESRI Shapefile]</a><br/>
      </td>
  </tr>
  <tr>
    <td class="tg-pcvp">Gebouwen in LoD1.0</td>
    <td class="tg-pcvp">Footprints van gebouwen met 1 hoogte per gebouw, berekend op basis van zowel het 75ste percentiel als 95ste percentiel van hoogtepunten die binnen het vlak vallen. Identiek aan versie 0.2.</td>
    <td class="tg-pcvp">&lt;tile id&gt;_lod10_&lt;percentile&gt;</td>
    <td class="tg-pcvp">
      <a href="{{ "lod10.zip" | prepend: "/download/noise3d/v03/" | prepend: site.baseurl }}">[ESRI Shapefile]</a><br/>
      </td>
  </tr>
  <tr>
    <td class="tg-0pky">Gebouwen in LoD1.3</td>
    <td class="tg-0pky">Footprints van gebouwen opgesplitst in dakdelen. Ieder dakdeel heeft een eigen hoogte gebaseerd op het 75ste percentiel van hoogtepunten punten die binnen het dakdeel vallen. De mininimale hoogtesprong tussen dakdelen is 3 meter (ongeveer 1 verdiepingshoogte).</td>
    <td class="tg-0pky">&lt;tile id&gt;_lod13_&lt;percentile&gt;</td>
    <td class="tg-0pky">
      <a href="{{ "lod13.zip" | prepend: "/download/noise3d/v03/" | prepend: site.baseurl }}">[ESRI Shapefile]</a><br/>
      </td>
  </tr>
</table>

### Attributen

In de attributen van de gebouwen is de volgende informatie opgenomen:

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;border:none;}
.tg td{padding:10px 5px;border-style:solid;border-width:0px;overflow:hidden;word-break:normal;}
.tg th{font-weight:normal;padding:10px 5px;border-style:solid;border-width:0px;overflow:hidden;word-break:normal;}
.tg .tg-fymr{font-weight:bold;border-color:inherit;text-align:left;vertical-align:top}
.tg .tg-pcvp{border-color:inherit;text-align:left;vertical-align:top;background-color: #ecf0f1;}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-fymr">Feature</th>
    <th class="tg-fymr">Attribuut</th>
    <th class="tg-fymr">Uitleg</th>
  </tr>
  <tr>
    <td class="tg-0pky">Gebouwen</td>
    <td class="tg-0pky">bag_id</td>
    <td class="tg-0pky">unieke <code>identificatie</code> in BAG-panden</td>
  </tr>
  <tr>
    <td class="tg-0pky"></td>
    <td class="tg-pcvp">dak_type</td>
    <td class="tg-pcvp">type of the roof of building<br>
      <code>2</code> – dak met tenminste één schuin vlak<br>
      <code>1</code> – dak met meerdere (en alleen maar) horizontale vlakken<br>
      <code>0</code> – dak met enkel een horizontaal vlak<br>
      <code>-1</code> – geen AHN-data beschikbaar binnen de gebouwomtrek<br>
      <code>-2</code> – wel AHN-data beschikbaar, maar er is geen dakoppervlak gedetecteerd
      </td>
  </tr>
  <tr>
    <td class="tg-0pky"></td>
    <td class="tg-0pky">rmse</td>
    <td class="tg-0pky"><em>Root mean square error</em> tussen het LoD1.3 model en de gedetecteerde dakpunten uit AHN3.</td>
  </tr>
  <tr>
    <td class="tg-0pky"></td>
    <td class="tg-0pky">h_dak</td>
    <td class="tg-0pky"><em>hoogte van het dakdeel</em> ten opzichte van NAP</td>
  </tr>
  <tr>
    <td class="tg-0pky"></td>
    <td class="tg-pcvp">h_maaiveld</td>
    <td class="tg-pcvp"><em>maaiveld hoogte</em> ten opzichte van NAP</td>
  </tr>
  <tr>
    <td class="tg-0pky"></td>
    <td class="tg-0pky">is_complex</td>
    <td class="tg-0pky">`1` als dit een complex gebouw betreft, dwz niet alle gedeteceerde daklijnen zijn meegenomen in de reconstructie om de verwerkingstijd te beperken.</td>
  </tr>
  <tr>
    <td class="tg-0pky"></td>
    <td class="tg-pcvp">ahn_geldig</td>
    <td class="tg-pcvp">Codering waarmee aangegeven wordt of de hoogte actueel is<br>
      <code>1</code> – gebouw is gebouwd <b>nadat</b> de AHN-puntenwolk is ingewonnen<br>
      <code>0</code> – gebouw is gebouwd <b>voordat</b> de AHN-puntenwolk is ingewonnen
    </td>
  </tr>
  <tr>
    <td class="tg-0pky" style="border-bottom-width:0.5px"></td>
    <td class="tg-0pky" style="border-bottom-width:0.5px">ahn_datum</td>
    <td class="tg-0pky" style="border-bottom-width:0.5px">inwindatum van de AHN-puntenwolk ter plaatse van het gebouw</td>
  </tr>
  <tr>
    <td class="tg-0pky">Bodemgebieden</td>
    <td class="tg-0pky">uuid</td>
    <td class="tg-0pky">unieke identificatie van het object</td>
  </tr>
  <tr>
    <td class="tg-0pky" style="border-bottom-width:0.5px"></td>
    <td class="tg-pcvp" style="border-bottom-width:0.5px">bodemfactor</td>
    <td class="tg-pcvp" style="border-bottom-width:0.5px">omschrijving van de bodemeigenschappen ("reflecterend" of "absorberend"), op basis van de BGT-classificatie, zoals aangegeven in bovenstaande tabel.<br>
      <code>0</code> – reflecterende bodem <br>
      <code>1</code> – absorberende bodem
    </td>
  </tr>
  <tr>
    <td class="tg-0pky">Hoogtebeschrijving terrein</td>
    <td class="tg-0pky">-</td>
    <td class="tg-0pky">In dit bestand zijn geen attributen aanwezig.</td>
  </tr>
</table>


## Disclaimer
Het bestand 3D geluid NL versie 0.2 wordt uitsluitend ter beschikking gesteld voor testdoeleinden. Er kunnen geen rechten aan worden ontleend.

Geen van de partijen die betrokken zijn bij de totstandkoming kan aansprakelijk worden gesteld voor eventuele schade die voortvloeit uit het gebruik van de data. 

----
# Project partners

{% include noise3d/partners.html %}
