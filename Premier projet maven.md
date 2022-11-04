
#1. Premier projet Maven avec Eclipse
- Ouvrir Eclipse
- File > New... > Projet.. > Maven > Maven project et Cliquer sur Next
- Cocher le "Create a simple project" et "Use default workspace locations" et Next
- Ensuite on a l'interface propre au projet Maven

<img title="Maven project" alt="Maven project" src="images/maven_prj.PNG">

Nous devons mettre tout d'abord:
* Group ID: le nom du package du projet. Il est important de comprendre que Maven utilise une convention pour les noms de  package (voir [ici](https://maven.apache.org/guides/mini/guide-naming-conventions.html)). Dans notre exemple, nous allons utiliser le nom `com.exomaven`
* Artifact Id: prend le nom du projet. dans notre cas, nous allons utiliser `first-maven-project`

Une fois rempli ces 2 �l�ments, nous allons clisuer sur `Finish`.


Le projet est alors construit et on retrouve une certaine structure comme suit:
![Alt text](images/first-maven_prj-structure.PNG)


Entre autres, on retrouve un fichier `pom.xml`. C'est la partie la plus importante d'un projet Maven.

Si on �dite le fichier, on retrouve les �l�ments que nous avons d�finie auparavant: `groupId` et  `artifactId` dedans.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.exomaven</groupId>
  <artifactId>first-maven-project</artifactId>
  <version>0.0.1-SNAPSHOT</version>
</project>
```

# 2 Une premi�re gestion de d�pendance avec Maven
Dans cette partie, nous allons cr�er une classe Java (tr�s simple) pour �muler l'utilisation de maven dans notre projet. 

Click-droit sur le r�pertoire `src/main/java` et cliquer sur `New > Class` (on a choisi `src`car c'est un fichier source code)

![Alt text](images/new_class.PNG)

Taper un nom de package `com.exomaven` (on a choisi le m�me nom que le groupid).

Taper dans `Name`, le nom de la classe `App` et cliquer `Finish`. 

Cette op�ration nous a cr�� un fichier App.java dans le projet et quelques autres packages aussi.

## 2.1 Cr�ation d'une fonction � tester pour notre projet

Dans la class `App`, rajoutons une fonction simple:
```java
public String returnSomething(){
    return "DUMMY";
}
```
Cela fait en sorte que nous avons un code source qui fait quelque chose (ici retourne le mot "DUMMY" mais cela pourrait �tre plus complexe)

## 2.2 Cr�ation d'une classe de test unitaire pour notre code source
Nous allons cr�er un test unitaire (junit) pour ce bout de code source. 

Cliquer-droit sur la racine du projet `File>New>Other...` et taper Junit.
Renseigner le m�me nom de `Package` que d�finit dans le code source: `com.exomaven`.
Renseigner �galement le champ `Name` par `Apptest`.

Cliquer sur `Finish` et une fen�tre appara�tra qui permettra de choisir si on doit modifier le `PATH` pour la librairie Junit. Pour l'instant, choisir `Not now` et `OK`.

Cela causera quelque erreurs de compilation car la d�pendance `Junit` n'est pas disponible alors que nous l'utilisons.

Dans un cas manuel (c'est � dire sans l'utilisation de Maven), nous pouvons g�rer nous-m�me les d�pendances. Par exemple, on pourrait t�l�charger le fichier jar de `Junit`, l'ajouter au `Build PATH` dans Eclipse et essayer de r�soudre les erreurs que nous avons. Si ensuite, nous partageons notre projet avec quelqu'un, il doit �galement refaire toute cette op�ration (t�l�charger le jar de Junit, param�trer correctement le `Build PATH` d'Eclipse) avant de pouvoir compiler. 

L'autre approche est d'utiliser Maven pour la gestion des d�pendances comme Junit. 

## 2.3 Ajout d'une d�pendance dans Maven
Editer le fichier `pom.xml` et nous allons rajouter une section `<dependancy>` en dessous de `<version>`. Pour cela, rajouter une balise 
```xml
<dependencies>    
    <dependency>
        On rajoute notre d�pendence ici
    </dependency>
</dependencies>
```
Dans la balise `<dependency>`,  il nous faut rajouter au moins 3 elements qui sont `<groupId>`, `<artifactId>` et `<version>` :
```xml
    <dependency>
        <groupId> </groupId>
        <artifactId> </artifactId>
        <version> </version>
    </dependency>
```
Nous allons rajouter ces �l�m�nts pour Junit. Une petite astuce, pour conna�tre le `<groupId>` de Junit pour Maven, il faut aller sur google et chercher `Junit maven dependecy` (si je choisi la sortie `junit � 4.13.2 - Maven Repository`). Cela nous trouvera le `<groupId>` de Junit: dans la section Maven. Je me contente de copier la balise `<dependency>` pour obtenir une configuration correcte de mon fichier `pom.xml`.

![Alt text](images/junit_dep.PNG)

*Remarque*: On peut se debarasser de la balise `<scope>` ou la garder.
 
La version finale de mon fichier `pom.xml` sera donc:

![Alt text](images/vers_pom_dep.PNG)


Maintenant que nous avons rajouter la d�pendance dans Maven, on remarquera que les erreurs de code Java caus� par Junit on disparus. Et que dans le projet, les dependances Maven sont maintenant renseign�s: 

![Alt text](images/maven_dep.PNG)

Si maintenant, sans avoir rien fait, nous allons lancer le test Junit, on peut l'executer. 

Pour executer le test Junit, cliquer sur le projet et `Run as... > Junit test` 

![Alt text](images/junit_launch.PNG). 

Le tests Junit se lancera mais on obtiendra un test qui est failed car le code du Test n'est pas impl�ment�:

```java 
@Test
	public void test() {
		fail("Not yet implemented");
	}
```
![Alt text](images/junit_failed.PNG)


On va r��crire le test comme suit, pour v�rifier que ce que retourne la fonction `App.returnSomething()` est �gale � `"DUMMY"`

```java 
@Test
	public void test1() {
        		assertTrue(App.returnSomething().equals("DUMMY"));
	}
```
On va �galement rajouter un deuxi�me test qui v�rifier que `1+1` est bien �gale � `2`

```java 
@Test
	public void test2() {
        		assertTrue(App.returnSomething().equals("DUMMY"));
	}
```

On va rajouter ces 2 fonctions dans le code des tests Junit, comme suit et relancer

![Alt text](images/source_test.PNG)

On obtient le r�sultat suivant:

![Alt text](images/junit_ok.PNG)