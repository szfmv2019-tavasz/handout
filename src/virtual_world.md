# Virtuális világ

Az autónak egy virtuális tesztpályán kell végigmennie, amelyhez modellezni a világot. Két teszt pályát biztosítunk, egy egyszerűbbet és egy nagyobbat, bonyolultabbat. Alább látható az egyszerűbb.

![](images/test_world.png)

A fenti világ elemekből épül föl, amelyek között vannak egyenes útszakaszok, kanyarok (pl. 45° és 90°), T elágazás, gyalogos átkelő, közúti táblák, parkolók és fák. Mindezt [egy XML írja le](resources/test_world.xml).

A számítógépes grafikában megszokott módon, a bal felső sarok jelenti az origót (0,0) koordinátát. Az x tengtely tehát jobbra haladva, az y tengely lefelé haladva növekszik. Így kell értelmezni az XML-t és ekképpen működik a megjelenítés is, hiszen a form bal felső sarkánál van a (0, 0) pont. Az alábbi kép [forrása](http://www.e-cartouche.ch/content_reg/cartouche/graphics/en/html/Screen_learningObject3.html).

![](http://www.e-cartouche.ch/content_reg/cartouche/graphics/en/image/coordinates.jpg)

## Villódzásmentes rajzolás

- https://docs.oracle.com/javase/tutorial/extra/fullscreen/doublebuf.html

## Objektumok leírása

Egy-egy objektum leírása a következőképpen néz ki:

``` xml
<Object type="road_2lane_straight">
      <Position x="1700" y="144"/>
      <Transform m11="0" m12="1" m21="-1" m22="0"/>
      <Parameter name="roadpainting_1" value="1"/>
      <Parameter name="roadpainting_2" value="1"/>
      <Parameter name="roadpainting_3" value="1"/>
</Object>
```

* A `type` attribútum írja le, hogy milyen objektumról van szó. A példában egy 2 sávos egyenes útszakaszról.
* A `Position` elem megadja az elem viszonyítási pontjának koordinátáját.
* A `Transform` elem egy 2×2-es transzformációs mátrix, amely forgatást ír le.
* A `roadpainting` megadná, hogy milyen felfestések vannak vannak az úton, de ez nem releváns
* Egy ilyen egyenes útelem 350×350px, ekkora a kép is és ennek kell lennie az XML-ben is.

## Az út elemek viszonyítási pontjai

![](images/90right.png) ![](images/90left.png)

![](images/45right.png) ![](images/45left.png)

![](images/6right.png) ![](images/6left.png)

![](images/tjunctionright.png)
![](images/tjunctionleft.png)

![](images/straight.png)
![](images/rotary.png)

![2_crossroad_1](images/2_crossroad_1.png)
![2_crossroad_2](images/2_crossroad_2.png)

Minden itt fel nem sorolt esetben feltételezhető, hogy a bal felső sarok a viszonyítási pont.

Egy korábbi félév során a referenciapontokat (részben) már összegyűjtötték (@csabalint, @SiposGergo, @markkurucz), ez elérhető [XML](references/reference_points.xml) vagy [JSON](references/reference_points.json) formátumban. 

A mozgatásra szoruló elemek (vezérelt autó és NPC autók) nem képezik részét a világ leírásának, viszonyítási pontjuk nem definiált. A rendelkezésre bocsátott autó képek méretarányosak az összes többi objektummal.

A világhoz szükséges elemek megtalálhatóak a project  `src/main/resources` mappájában.

## Méretarány

Az XML-ben leírt objektumok koordinátarendszere nem feltétlenül egyezik meg a megjelenítő koordinátarendszerével, ezt figyelembe véve skálázás, vagy viewport kezelés válhat szükségessé.

A feladat megoldás során jellemzően valóságos mértékegységekben specifikáljuk a feladatot (pl. méter, km/h, m/s^2, stb.) míg az XML és a grafikai elemek értelem szerűen pixellel dolgoznak. Ennek feloldására, illetve az átváltásra az **1m=50px** szabályt célszerű használni. Ez egy hozzávetőlegesen arányos érték, amellyel számolni is könnyű.

## Sávtartó automatika tesztelése

A sávtartó automatika nem tesztelhető kanyarokban, ehhez ezért egy alább látható kinézetű pályaszakasz készült. Csak 6°-os és egyenes útszakaszból áll. [Az XML elérhető itt](resources/lane_keeping_test_world.xml).

![](images/lka_track.png)
