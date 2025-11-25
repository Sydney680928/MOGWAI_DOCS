# GLOSSAIRE

## LES FONCTIONS DU LANGAGE

### `mogwai.doworks`

Force **MOGWAI** à traiter les événements et les timers en attente.
***

### `mogwai.reset`

Force **MOGWAI** à faire une remise à zéro du runtime.
***

### `mogwai.info`

Retourne un record contenant toute une série d'informations concernant le runtime et le système sur lequel il tourne.

```
mogwai.info ?d
```

Affichera :

```
name:             "MOGWAI CLI"
version:          "6.48.0.0"
platform:         "WINDOWS"
architecture:     "X64"
OSdescription:    "Microsoft Windows 10.0.26200"
framework:        ".NET 7.0.20"
runtimeID:        "win10-x64"
prompt:           "MOGWAI RUNTIME 6.48.0.0 © 2016-2025 Stéphane SIBU ...
keywords:         ('mogwai.doworks' 'mogwai.reset' 'mogwai.info' 'mo ...
skills:           ("SQLDB" "SERIAL")
extensions:       ()
debug:            false
keepAlive:        true
isTask:           false
fileSupport:      true
```

| Clé | Signification |
|-----|---------------|
| `name:` | Nom du runtime.|
| `version:` | Version du runtime.|
| `platform:` | Nom de la plateforme sur laquelle le runtime tourne.|
| `architecture:` | Architecture de la plateforme.|
| `OSdescription:` | Description complète de la plateforme.|
| `framework:` | Version du runtime .NET |
| `runtimeID:` | ID du runtime de la plateforme.|
| `prompt:` | Prompt du runtime **MOGWAI**.|
| `skills:` | Liste des skills du runtime **MOGWAI**.|
| `extensions:` | Liste des extensions chargées.|
| `keepAlive:` | true si le runtime **MOGWAI** garde d'une session à une autre son contexte d'exécution.|
| `isTask:` | true si le runtime **MOGWAI** est une tâche fille.|
| `fileSupport:` | true si le runtime **MOGWAI** peut effectuer des opérations sur les fichiers.|
***

### `mogwai.exit`

Force le runtime à stopper l'exécution en cours sans lever d'erreur.
***

### `mogwai.halt`

Force le runtime à stopper l'exécution en cours et lève une erreur 17 "halt encounted".
***

### `mogwai.cclear`

Efface le cache des procédures incluses via la commande `include`.
<br>Permet de s'assurer que le code inclus est bien la toute dernière version.
***

### `mogwai.memory`

Retourne la quantité de mémoire allouée par le système pour le runtime.
***

### `mogwai.explicit`

Si `true` est passé en paramètre, toutes les variables doivent être déclarées avant d'être utilisées.
<br>Les variables sont déclarées avec la fonction `def`.
>Par défaut `mogwai.explicit` est désactivé. 
***

### `mogwai.isTask`

Retourne `true` si le runtime est une tâche fille (voir la gestion des tâches).
***

### `env.machineName`

Retourne le nom de la machine sur laquelle le runtime fonctionne sous la forme d'une chaîne de caractères.
***

### `reveal`

Prend une fonction utilisateur en paramètre (nom ou code) et retourne la version 100% RPN correspondante. Cette version 100% RPN sera le code réellement utilisé pour l'exécution (tout le sucre syntaxique transformé en code final).

```
to 'essai' do « 1 10 for 'i' do { i ? } »

'essai' reveal ?

# Affichera le code suivant :
# « 1 10 'i' { i ? } FOR »
```
***

### `args`

Retourne la liste des arguments passés au moteur lors de sa création par le host sous la forme d'une liste de chaînes de caractères.
***
 
### `defunc`

Fonction 100% RPN qui permet la déclaration d'une fonction.

```
to 'carre' do « dup * »

# Est identique à

« dup * » 'carre' defunc
```
***

### `funcs`

Retourne la liste des fonctions utilisateur définies.
***

### `def`

Permet de déclarer une variable avant de l'utiliser.

Si `mogwai.explicit` est activé il est obligatoire de déclarer les variables avant de les utiliser (par défaut non).

Quand une variable est déclarée explicitement, elle garde toujours le type qui lui est affecté et ne peut plus changer de type à la volée à moins d'utiliser le type `.any`

Pour déclarer une variable il faut donner son type et son nom à la fonction `def`.

```
.string 'maString' def
.number 'monNombre' def
```
***

### `const`

Permet de déclarer une constante. Sa valeur est immuable.

```
500 'MA_CONSTANTE' const
```
***

### `sto` ou `->`

Stocke une valeur dans une variable.

```
50 'A' sto
```

Une version édulcorée existe, plus lisible :

```
50 -> 'A'
```
***

### `sto+` ou `->+`
Ajoute une valeur à une variable.

```
50 'A' sto+
```

Version édulcorée :

```
50 ->+ 'A'
```
***

### `sto-` ou `->-`

Soustrait une valeur à une variable.

```
50 'A' sto-
```

Version édulcorée :

```
50 ->- 'A'
```
***

### `sto*` ou `->*`

Multiplie une variable avec une valeur :

```
50 'A' sto*
```

Version édulcorée :

```
50 ->* 'A'
```
***

### `sto/`ou `->/`

Divise une variable par une valeur :

```
50 'A' sto/
```

Version édulcorée :

```
50 ->/ 'A'
```
***

### `++`

Incrémente une variable.

```
'A' ++
```
***

### `--`

Décrémente une variable.

```
'A' --
```
***

### `apply` ou `-->`

Applique un traitement à une variable.

Par exemple, `A 1 + -> 'A'` peut être remplacé par `{ 1 + } --> 'A'` ou `{ 1 + } 'A' apply`
***

### `rcl`

Pose sur la pile la valeur d'une variable dont le nom est passé en paramètre.

```
100 -> '$A'
'$A' rcl ?

# Affiche 100
```
***

### `purge`

Supprime une variable dont le nom est passé en paramètre.

```
'$A' purge
```
***

### `exists`

Retourne `true` si la variable dont le nom est passé en paramètre existe.

```
'$A' exists
```
***

### `eval`

Evalue un objet posé sur la pile.

Le comportement est différent suivant le type d'objet évalué :
- Les fonctions et blocs de code sont exécutés. 
- Les chaînes de caractères sont mises à jour avec les caractères de contrôle et les blocs de remplacement.
- Les éléments dynamiques d'une liste sont remplacés par leur valeur actuelle.
- Les éléments dynamiques d'un record sont remplacés par leur valeur actuelle.

```
"Monsieur X" -> 'name'
"Le nom est {! Name}" eval ?

# Affiche "Le nom est Monsieur X"

[x: 50 name: name] eval

# Pose sur la pile [x: 50 name: "Monsieur X"]

```
***

### `include`

Inclut et exécute immédiatement du code provenant d'un fichier.

```
"mon code.mog" include
```
***

### `import`

Importe une bibliothèque d'extension au format ***MOGWAI***.

C'est un fichier .dll (une bibliothèque .NET) mais on indique juste le nom du fichier sans l'extension.

```
"maths" import
```
***

### `imports`

Liste les imports effectués et disponibles.
***

### `import.keywords`

Liste toutes les fonctions disponibles dans une extension chargée (par la fonction import).
***

### `get`

Retourne la valeur d'une clé dans un record, d'un élément d'une liste ou d'un data :

| Action | Résultat |
|--------|----------|
| `(1 2 3 4) 1 get` | retournera 2|
| `[x: 10 y: 20] x: get` | retournera 10|
| `[x: 10 l: (1 2 3)] (l: 1) get` | retournera 2|
| `DATA:FFEA10 1 get` | retournera 234 (0xEA)|

Retourne la valeur d'une propriété d'un objet, ou exécute la méthode d'un objet :

| Action | Résultat |
|--------|----------|
| `@1 name: get` | retournera la valeur de la propriété 'name' de l'objet @1.|
| `@1 display: get` | exécutera la fonction display (si display est une méthode) de l'objet @1.|
***

### `set`

Modifie la valeur d'une clé dans un record, liste ou un data :

| Action | Résultat |
|--------|----------|
| `(1 2 3 4) 0 10 set` | retournera `(10 2 3 4)`|
| `[x: 10 y: 20] x: 100 set` | retournera `[x: 100 y: 20]`|
| `DATA:FFEA10 0 0xAA set` | retournera `DATA:AAEA10`|

Modifie la valeur d'une propriété d'un objet :

| Action | Résultat |
|--------|----------|
| `@1 name: "DUPONT" set` | affectera "DUPONT" à la propriété 'name' de l'objet @1.|
***

### `size`

Retourne la taille d'un record, d'une liste, d'un data, d'un binaire ou d'une chaîne de caractères.
***

### `resize`

Modifie la taille d'un binaire.

```
BIN:11011 8 resize

# Posera sur la pile BIN:00011011
```
***

### `keys`

Retourne une liste composée des clés d'un record.

```
[x: 10 y: 50 z: 100] keys 

# Posera sur la pile (x: y: z:)
```
***

### `first`

Retourne le 1er élément d'une chaîne de caractères, d'une liste ou d'un data.
***

### `last`

Retourne le dernier élément d'une chaîne de caractères, d'une liste ou d'un data.
***

### `butfirst`

Retourne tous les éléments sauf le 1er d'une chaîne de caractères, d'une liste ou d'un data.
***

### `butlast`

Retourne tous les éléments sauf le dernier d'une chaîne de caractères, d'une liste ou d'un data.
***

### `contains`

Retourne `true` si un élément est présent dans une chaîne de caractères, un record, une liste ou un data.

| Action | Résultat |
|--------|----------|
| `"TOTO" "T" contains` | retournera `true`.|
| `[x: 50 y: 100] x: contains` | retournera `true`.|
| `(10 "EEE" 20 50) 20 contains` | retournera `true`.|
| `DATA:FF00FFAB 0xFF contains` | retournera `true`.|
***

### `where`

Retourne  une liste de tous les emplacements d'un élément dans une chaîne de caractères, une liste ou un data.

| Action | Résultat |
|--------|----------|
| `"BONJOUR LE MONDE" "O" where` | retournera `(1 4 12)`|
| `(10 100 40 10 24) 10 where` | retournera `(0 3)`|
| `DATA:45ED23FF0645DD 0x45` | where retournera `(0 5)`|
***

### `split`

Retourne une liste composée des éléments d'une chaîne de caractères séparés par une chaîne contenant le séparateur (qui peut être composé de plusieurs caractères).

| Action | Résultat |
|--------|----------|
| `"X1;X45;Z34;12" split` | retournera `("X1" "X45" "Z34" "12")`|
***

### `join`

Recompose une chaîne à partir des éléments d'une liste et d'un séparateur.

> Fonction inverse de split.

| Action | Résultat |
|--------|----------|
| `("X1" "X45" "Z34" "12") ";" join` | retournera `"X1;X45;Z34;12"`|

### `like`

Retourne `true` si une chaîne de caractères répond à un pattern particulier.

```
?           = Tout caractère unique
*           = Zéro ou plusieurs caractères
#           = N’importe quel chiffre (0 à 9)
[charlist]  = N’importe quel caractère unique dans charlist
[!charlist] = N’importe quel caractère unique qui n’est pas dans charlist
```

```
"MR DUPONT 62" "M? DU*T ??" like

# Pose true sur la pile
```
***

### `right`

Retourne les n derniers caractères d'une chaîne de caractères.

```
"Bonjour le monde !" 6 right

# Pose " onde !" sur la pile
```
*** 

### `left`

Retourne les n premiers caractères d'une chaîne de caractères.

```
"Bonjour le monde !" 6 left

# Pose "Bonjou" sur la pile
```
***

### `extract`

Extrait plusieurs éléments d'une liste ou d'un data en indiquant quels éléments extraire dans une liste.

```
(10 20 30 40) (0 1 3) extract 

# retournera (10 20 40)

DATA:FF45AB23 (0 1 3) extract

# retournera DATA:FF4523

[x: 10 y: 20 z: 100] (x: z:) extract 

# retournera [x: 10 z: 100]
```
***

### `sleep`

Suspend le runtime pendant un temps exprimé en millisecondes. 
> Attention, sleep bloque le traitement de tous les messages de type événements et timers.
***

### `wait`

Comme `sleep`, suspend le runtime un temps exprimé en millisecondes sans bloquer le traitement des messages de type événements et timers.
***

### `rand`

Retourne un nombre aléatoire compris entre 0 et 1.
***

### `sub`

Extrait une partie d'une liste ou d'un data en stipulant le début et l'étendue.

```
(10 20 30 40 50) 1 3 sub
# Pose sur la pile (20 30 40)

DATA:05FFEDAB2312 2 3 sub 
# Pose sur la pile DATA:EDAB23

BIN:1001111001111 2 4 sub 
# Pose sur la pile BIN:0011
```
***

### `break`

Force la sortie d'une boucle for, while, foreach, forever et during.
***

### `return`

Force la sortie d'une fonction.
***

### `run`

Lance l'exécution d'un programme dont le code se trouve dans un fichier.

```
"mon programme.mog" run
```
***

### `new`

Pose sur la pile un objet ***MOGWAI*** en stipulant son type.

```
.list new
# Pose sur la pile une liste vide.

.string new 
# Pose sur la pile une chaîne de caractères vide.

.record new 
# Pose sur la pile un record vide.
```
***

### `isSupported`

Retourne `true` si une fonction dont le nom est passé en paramètre est supportée par la plateforme courante.

```
'sto' isSupported 
# Pose sur la pile true.

'serial.open' iSupported 
# Pose sur la pile true sous windows, osx et linux.
```
***

### `isSkill`

Retourne `true` si le skill dont le nom est passé en paramètre est supporté par le runtime.

```
"SQLDB" isSkill
# retournera true si le support SQLite est activé.
```
***

### `skills`

Retourne la liste des skills du runtime. Chaque skill est fourni sous la forme d'une chaîne de caractères.
***

### `flags`

Retourne la liste de tous les flags actifs.
***

### `flag.set`

Active le flag dont le nom est passé en paramètre.
***

### `flag.clear`

Désactive le flag dont le nom est passé en paramètre.
***

### `flag.isSet`

Retourne true si le flag dont le nom est passé en paramètre est actif.
***

### `flag.isClear`

Retourne true si le flag dont le nom est passé en paramètre est inactif.
***

### `unique`

Retourne un code unique sous la forme d'une chaîne de caractères.
<br>Ex: "DEC378AF69F246B6A1688799F70A987A"
***

### `guid`

Retourne un code unique au format UUID (ou GUID) sous la forme d'une chaîne de caractères.
<br>Ex: "392BDA7A-9BEB-43B2-ACC7-05C8A06B0F44"
***

### `json->`

Crée une liste ou un record à partir d'une chaîne de caractères au format json.
***

### `->json`

Crée une chaîne de caractères au format json à partir d'une liste ou d'un record.
***

### `escape`

Escape une chaîne de caractères passée en paramètre.

Les guillemets sont remplacés par `\"`, les retours à la ligne par des `\r` et/ou des `\n`, etc… 
***

### `help`

Retourne une chaîne de caractères qui contient l'aide d'une fonction dont le nom est passé en paramètre.

> Très peu de fonctions peuvent répondre à cette commande pour le moment (mais ça va changer).

```
'sto' help ?
```

Affichera par exemple :

```
*********************
** sto (primitive) **
*********************
Stocke une valeur dans une variable.
50 'A' sto
"Bonjour" 'message' sto
(1 2 3) 'maliste' sto
```
***

### `?help`

Affiche l'aide d'une fonction dont le nom est passé en paramètre.
***

### `error.last`

Retourne le code de la dernière erreur levée.
***

### `error.reset`

Remet à zéro le code de la dernière erreur levée.
***

### `error.throw`

Lève artificiellement l'erreur dont le code est passé en paramètre.
***

### `+`

Additionne 2 objets entre eux.

Les combinaisons possibles sont :
- 2 nombres
- 1 liste et un objet
- 2 chaînes de caractères
- 1 data et un byte
- 2 data
- 2 listes
***

### `-` 

Soustrait 2 nombres.
***

### `*`

Multiplie 2 nombres.
***

### `/`

Divise 2 nombres.
***

### `<`

Retourne `true` si le 1er paramètre est inférieur au second.
***
 
### `>`
Retourne `true` si le 1er paramètre est suppérieur au second.
***

### `<=`
Retourne `true` si le 1er paramètre est inférieur ou égal au second.
***

### `>=`
Retourne `true` si le 1er paramètre est suppérieur ou égal au second.
***

### `==`
Retourne `true` si le 1er paramètre est égal au second.
***

### `!=`
Retourne `true` si le 1er paramètre est différent au second.
***

### `and`
Effectue l'opération logique AND entre le 1er et le second paramètre.
***

### `or`
Effectue l'opération logique OR entre le 1er et le second paramètre.
***

### `xor`
Effectue l'opération logique XOR entre le 1er et le second paramètre.
***

### `not`
Effectue l'opération logique NOT entre le 1er et le second paramètre.
***

### `isnull`
Retourne `true` si l'objet passé en paramètre est `null`.
***

### `isRecord`
Retourne `true` si l'objet passé en paramètre est de type `.record`
***

### `isList`
Retourne `true` si l'objet passé en paramètre est de type `.list`
***

### `isData`
Retourne `true` si l'objet passé en paramètre est de type `.data`
***

### `isBoolean`
Retourne `true` si l'objet passé en paramètre est de type `.boolean`
***

### `isNumber`
Retourne `true` si l'objet passé en paramètre est de type `.number`
***

### `isString`
Retourne `true` si l'objet passé en paramètre est de type `.string`
***

### `isName`
Retourne `true` si l'objet passé en paramètre est de type `.name`
***

### `isKey`
Retourne `true` si l'objet passé en paramètre est de type `.key`
***

### `drop`
Supprime le 1er élément de la pile.
***

### `drop2`
Supprime les 2 premiers éléments de la pile.
***

### `dropn`
Supprime les n derniers éléments de la pile. Le nombre d'éléments à supprimer est passé en paramètre.
***

### `swap`
Intervertit les 2 premiers éléments de la pile.
***

### `dup`
Duplique le 1er élément de la pile.
***

### `dup2`
Duplique les 2 premiers éléments de la pile.
***

### `dupn`
Duplique les n derniers éléments de la pile. Le nombre d'éléments à supprimer est passé en paramètre.
***

### `pick`
Retourne la valeur du nème élément de la pile (0 étant le 1er élément) sans le retirer de la pile.
***

### `over`
Prend le 2ème élément de la pile (sans le retirer) et l'ajoute à la pile.
***

### `roll`
Retire le nème élément de la pile (passé en paramètre) et l'ajoute à la pile.
***

### `rolld`
Retire le 1er élément de la pile et l'insère à l'emplacement passé en paramètre.
***

### `rot`
Inverse le 1er et le 3ème élément de la pile.
***

### `depth`
Retourne le nombre d'éléments dans la pile.
***

### `clear`
Efface la pile.
***

### `sign`
Retourne une liste contenant le type des n éléments de la pile sans modifier la pile.

```
# On place des éléments sur la pile
10 "EEE"

# On demande le type de ces 2 élements
2 sign

# La liste (.string .number) est posée sur la pile
```
***

### `->type`
Retourne le type de l'objet passé en paramètre.
***

### `compress`
Retourne un data qui est le résultat de la compression d'un data passé en paramètre.
***

### `decompress`
Retourne un data qui est le résultat de la décompression d'un data passé en paramètre. Le data passé en paramètre est normalement le résultat de la fonction `compress`.
***

### `vars`
Retourne la liste de toutes les variables globales existantes.
***

### `lvars`
Retourne la liste de toutes les variables locales existantes.
***

## `IF`
Fonction 100% RPN correspondant au `if` édulcoré.

```
{ A 100 == } { "A est égal à 100" ? } IF

# Si A a pour valeur 100, affichera "A est égal à 100"
# sinon ne fera rien.
```
***

### `IFELSE`
Fonction 100% RPN correspondant au `if … else` édulcoré.

```
{ A 100 == } { "A est égal à 100" ? } { "A est différent de 100" ? } IFELSE

# Si A a pour valeur 100, affichera "A est égal à 100"
# sinon affichera "A est différent de 100"
```
***

### `SWITCH`
Fonction 100% RPN correspondant au `switch` édulcoré.

```
{
    { A 100 == } { "A = 100" ? } 
    
    { A 100 > } { "A > à 100 " ? }

} SWITCH
```
***

### `REPEAT`
Fonction 100% RPN correspondant au `repeat` édulcoré.

```
3 { "Bonjour !" } REPEAT
```
***

### `WHILE`
Fonction 100% RPN correspondant au `while` édulcoré.

```
{ A 10 < } { A ? 'A'  ++ } WHILE
```
***

### `DOWHILE`
Fonction 100% RPN correspondant au `do … while` édulcoré.

```
{ A 10 < } { A ? 'A' ++ } DOWHILE
```
***

### `FOREACH`
Fonction 100% RPN correspondant au `foreach` édulcoré.

```
(1 2 3 4) 'A' { A ? } FOREACH
```
***

### `FOREVER`
Fonction 100% RPN correspondant au `forever` édulcoré.

```
{ A ? 'A' ++ } FOREVER
```
***

### `FOR`
Fonction 100% RPN correspondant au `for` édulcoré.

```
1 10 'A' { A ? } FOR
```
***

### `FORSTEP`
Fonction 100% RPN correspondant au `for … step` édulcoré.

```
1 100 5 'A' { A ? } FORSTEP
```
***

### `DURING`
Fonction 100% RPN correspondant au `during` édulcoré.

```
2500 { A ? 'A' ++ } DURING
```
***

### `LATER`
Fonction 100% RPN correspondant au `after` édulcoré.

```
5000 « "Hello" ? » LATER { } FOREVER
```
***

### `GUARD`
Fonction 100% RPN correspondant au `guard … else` édulcoré.

```
{ "MonFichier" file.read } { "Erreur d'accès du fichier !" ? } GUARD
```
***

### `TRAP`
Fonction 100% RPN correspondant au `trap` édulcoré.

```
{ "MonFichier" file.read } TRAP
```
***

### `print` ou `??`
Affiche une chaîne de caractères à l'écran sans revenir à la ligne.

```
"Bonjour " print "le monde !" println

# Affichera :
# Bonjour le monde !
```
***

### `println ou `?`
Affiche une chaîne de caractères à l'écran en revenant à la ligne.

***

### `?vars` ou `?v`
Affiche à l'écran la liste des variables globales avec une présentation "claire".

```
100 -> '$A' "HELLO" -> '$B' (10 20 30) -> '$C' ?vars
```
Affichera :

```
$A = 100
$B = "HELLO"
$C = (10 20 30)
```
***

### `?stack` ou `?s`
Affiche la pile à l'écran. Le 1er élément étant affiché en dernier.

```
10 20 30 40 50 ?stack
```

Affichera :

```
4 : 10
3 : 20
2 : 30
1 : 40
0 : 50
```
***
 
### `?dump` ou `?d`
Affiche à l'écran les listes, les records et les data dans une version plus "claire".

```
(10 20 30 40 50) ?dump
```

Affichera :

```
0 : 10
1 : 20
2 : 30
3 : 40
4 : 50
```

```
[x: 100 y: 50 z: "HELLO"] ?dump
```

Affichera :

```
x:             100
y:             50
z:             "HELLO"
```

```
DATA:5612FFEA1789AD34C5FAFEFF01021020ABACA0 ?dump
```

Affichera :

```
00000000  56 12 FF EA 17 89 AD 34 C5 FA FE FF 01 02 10 20  | V.ÿê.?­4Åúþÿ...   |
00000010  AB AC A0
```
***

### `cls`
Efface l'écran.
***

### `input`
Se met en attente d'une entrée au clavier (se terminant par une validation par la touche `ENTER`) et retourne la chaîne de caractères correspondante.
***

### `prompt`
Comme la fonction `input` mais affiche un message d'invite (prompt) passé en paramètre.

```
"Quel est votre prénom ? " prompt
"Votre prénom est : " swap + ?
```

Affichera le prompt, puis on pourra saisir (par exemple STEPHANE)

```
Quel est votre prénom ? STEPHANE
```

Puis une fois la saisie (STEPHANE) validée :

```
Votre prénom est : STEPHANE
```
***

### `console.show`
Affiche la console de sortie (si géré par le host).
> N'a pas d'effet dans **MOGWAI CLI**.
***

### `console.hide`
Cache la console de sortie (si géré par le host).
> N'a pas d'effet dans **MOGWAI CLI**.
***

### `->list`
Construit une liste à partir des éléments présents dans la pile. Il faut passer en paramètre le nombre d'éléments à prendre. Une erreur est levée si la pile ne contient pas assez d'éléments.

```
10 20 30 40 50 5 ->list ?

# Pose sur la pile (10 20 30 40 50)
```
***

### `->int`
Convertit un nombre passé en paramètre en entier
***

### `->str`
Convertit un objet passé en paramètre en chaîne de caractères.
***

### `->format`
Convertit un nombre en chaîne de caractères en utilisant un format.

```
50 "000" ->format ?
# Affichera 050

50.8 "000.000" ->format ?
# Affichera 050.800
```
***

### `->vars`
Extrait des valeurs et les affecte à des variables créées localement.

Avec un record, extrait les valeurs de toutes les clés et crée les variables locales correspondantes aux clés extraites :

```
[x: 10 y: 20 z: 50] ->vars 
"x={! x}" eval ? 
"y={! y}" eval ? 
"z={! z}" eval ?
```

Affichera :

```
x=10
y=20
z=50
```
***

Avec la pile, extrait les valeurs et crée les variables locales correspondantes :

```
20 30 40 ('a' 'b' 'c') ->vars 
"a={! a}" eval ? 
"b={! b}" eval ? 
"c={! c}" eval ?
```

Affichera :

```
a=20
b=30
c=40
```
***

### `->safeVars`
Vérifie que les valeurs présentes sur la pile sont bien celles attendues. 
Vous pouvez vérifier leur nombre et leur type, et affecter automatiquement des variables locales avec les valeurs de la pile. En cas de non-conformité une erreur est levée.

```
"EEE" 50 [x: .string y: .number] ->safeVars 
"x={! x}" eval ? 
"y={! y}" eval ?
```

Affichera :

```
x=EEE
y=50
```
***

### `->params`
Permet de passer des paramètres nommés (des clés/valeurs dans un record) et de vérifier que les paramètres attendus sont bien présents et que leur type correspond. 
Si tout est correct, les variables locales correspondantes aux paramètres attendus sont automatiquement créées avec les valeurs correspondantes.

```
[x: 100 y: "HELLO"] [x: .number y: .string] ->params 
"x={! x}" eval ? 
"y={! y}" eval ?
```

Affichera :

```
x=100
y=HELLO
```
***

### `->num`
Convertit une chaîne de caractères en nombre quand c'est possible. 
Si impossible une erreur est levée.
***

### `->name`
Convertit une chaîne de caractères ou une clé en nom.
***

### `->key`
Convertit une chaîne de caractères ou un nom en clé.
***

### `->data`
Prélève n éléments de la pile et en fait un data. 
Le nombre d'éléments à prélever est passé en paramètre.

```
0xFF 0xAB 0x45 3 ->data

# Pose sur la pile DATA:FFAB45
```
***

### `->hex`
Convertit un nombre en chaîne de caractères au format hexadécimal.

```
255 ->hex 

# Pose sur la pile "FF"
```
***

### `hex->`
Convertit une chaîne de caractères au format hexadécimal en nombre.

```
"FF" hex->

# Pose sur la pile le nombre 255
```
***

### `->bin`
Convertit un nombre en objet binaire.

```
278 ->bin

# Pose sur la pile BIN:100010110
```
***

### `->upper`
Convertit une chaîne de caractères en majuscules.
***

### `->lower`
Convertit une chaîne de caractères en minuscules.
***

### `->program`
Convertit une liste en fonction.

```
( 2 2 + ) ->program

# Pose sur la pile « 2 2 + »
```
***

### `->keyword`
Convertit une chaîne de caractères en nom de fonction **MOGWAI** (mot clé).

>Attention, le mot clé est placé sur la pile et n'est pas automatiquement exécuté.<br>
Pour l'exécuter il faut utiliser la fonction eval.

```
10 '$A' "sto" ->keyword eval
$A

# Revient à écrire :
# 10 '$A' sto 
# $A
# Qui pose sur la pile le contenu de $A, le nombre 10
```
***

### `->u8`
Convertit un nombre en entier 8 bits non signé. 
Le résultat est retourné sous la forme d'un data.
***

### `->i8`
Convertit un nombre en entier 8 bits signé. 
Le résultat est retourné sous la forme d'un data.
***

### `->u16`
Convertit un nombre en entier 16 bits non signé. 
Le résultat est retourné sous la forme d'un data.
***

### `->i16`
Convertit un nombre en entier 16 bits signé. 
Le résultat est retourné sous la forme d'un data.
***

### `->u32`
Convertit un nombre en entier 32 bits non signé. 
Le résultat est retourné sous la forme d'un data.
***

### `->i32`
Convertit un nombre en entier 32 bits signé. 
Le résultat est retourné sous la forme d'un data.
***

### `->u64`
Convertit un nombre en entier 64 bits non signé. 
Le résultat est retourné sous la forme d'un data.
***

### `->i64`
Convertit un nombre en entier 64 bits signé. 
Le résultat est retourné sous la forme d'un data.
***

### `->utf8`
Convertit un data en chaîne de caractères encodé en UFT-8.
***

### `->ascii7`
Convertit un data en chaîne de caractères encodé en ASCII 7 bits.
***

### `->ascii`
Convertit un data en chaîne de caractères encodé en ASCII 8 bits.
***

### `->base64`
Convertit un data en chaîne de caractères encodé en base 64.
***

### `->md5`
Retourne le hash md5 d'un data. 
Le hash est fourni sous la forme d'un data.
***

### `->sha1`
Retourne le hash sha1 d'un data.
Le hash est fourni sous la forme d'un data.
***

### `shift`
Effectue un décalage de bits sur un nombre ou un objet binaire. 
Le décalage est passé en paramètre. 
Un décalage positif décale les bits vers la droite, négatif vers la gauche.

```
500 2 shift
# Pose sur la pile 125

BIN:01101111 2 shift
# Pose sur la pile BIN:00011011

BIN:01101111 -2 shift
# Pose sur la pile BIN:10111100
```
***

### `~`
Inverse chaque bit d’un nombre passé en paramètre.
***

### `&`
ET binaire entre 2 nombres passés en paramètres.
***

### `|`
OU binaire entre 2 nombres passés en paramètres.
***

### `^`
XOR binaire entre 2 nombres passés en paramètres.
***

### `reverse`
Inverse l’ordre des éléments d’une liste, d’un data, d’un objet binaire ou d’une chaîne de caractères.

```
DATA:A0B5 reverse
# Pose sur la pile DATA:B5A0

(1 2 3) reverse
# Pose sur la pile (3 2 1)

"ABC" reverse
# Pose sur la pile "CBA"

BIN:000111 reverse
# Pose sur la pile BIN:111000
```
***

### `up`
Lève un bit particulier d’un objet binaire.

```
BIN:110001 2 up
# Pose sur la pile BIN:110101
```
***

### `down`
Tombe un bit particulier d’un objet binaire.

```
BIN:110101 2 down
# Pose sur le pile BIN:110001
```
***

### `classes`
Retourne la liste de toutes les classes utilisateur définies.
***

### `defClass`
Permet de définir une classe.
***
 
### `ALLOC`
Fonction 100% RPN de `alloc … to …` édulcoré.

```
'MaClasse' '$MaVariable' ALLOC

# Crée une nouvelle instance de classe 'MaClasse'
# et stocke sa référence dans la variable '$MaVariable'
```
***

### `props`
Retourne la liste des propriétés d’une instance de classe passée en paramètre.
***

### `?instances`
Liste toutes les instances déclarées (avec `alloc` ou `ALLOC`) avec leur compteur d’utilisation. 
Quand le compteur arrive à zéro l’instance est automatiquement supprimée. 
> Cette fonction est utilisée pendant la mise au point de votre programme.
***

### `sin`
Retourne le sinus d’un angle passé en paramètre. L’angle est en radian.
***

### `cos`
Retourne le cosinus d’un angle passé en paramètre. L’angle est en radian.
***

### `tan`
Retourne la tangente d’un angle passé en paramètre. L’angle est en radian.
***

### `asin`
Retourne l’angle en radian dont le sinus est passé en paramètre.
***

### `acos`
Retourne l’angle en radian dont le cosinus est passé en paramètre.
***

### `atan`
Retourne l’angle en radian dont la tangente est passée en paramètre.

### `PI`
Retourne le nombre PI.
***

### `->deg`
Retourne l’angle en degré d’un angle en radian passé en paramètre.

```
PI 3 / ->deg
# Pose sur la pile 60
```
***

### `->rad`
Retourne l’angle en radian d’un angle en degré passé en paramètre.

```
60 ->rad
# Pose sur la pile 1.0471975511965976
```
***

### `abs`
Retourne la valeur absolue du nombre passé en paramètre.
***

### `sqrt`
Retourne la racine carrée du nombre passé en paramètre.
***

### `floor`
Retourne la plus grande valeur intégrale inférieure ou égale au nombre passé en paramètre.
***

### `pow`
Retourne un nombre passé en paramètre élevé à la puissance passée en paramètre.

```
50 3 pow
# Pose sur la pile 125000
```
***

### `mod`
Retourne le reste de la division entière d’un nombre par un autre.

```
65 3 mod ?
# Pose sur la pile 2
```
***

### `min`
Retourne le plus petit nombre présent dans une liste. 
> Seuls les nombres sont pris en compte, si d’autres types sont présents dans la liste ils sont ignorés.

```
(1 56 34 9 27) min
# Pose sur la pile 1
```
***

### `max`
Retourne le plus grand nombre présent dans une liste. 
> Seuls les nombres sont pris en compte, si d’autres types sont présents dans la liste ils sont ignorés.

```
(1 56 34 9 27) max
# Pose sur la pile 56
```
***

### `sum`
Retourne la somme de tous les nombres présents dans une liste.
> Seuls les nombres sont pris en compte, si d’autres types sont présents dans la liste ils sont ignorés.

```
(1 56 34 9 27) sum
# Pose sur la pile 127
```
***

### `mean`
Retourne la moyenne de tous les nombres présents dans une liste.
> Seuls les nombres sont pris en compte, si d’autres types sont présents dans la liste ils sont ignorés.

```
(1 56 34 9 27) mean ?
# Pose sur la pile 25.4
```
***

### `console.locate`
Demande au host du runtime de positionner le curseur aux coordonnées passée en paramètre. 
Le host n’est pas obligé d’y répondre. 
> MOGWAI CLI gère cette fonction.

```
5 7 console.locate
```
***

### `console.cursor`
Retourne les coordonnées actuelles du curseur dans l’écran du host. 
Si le host ne gère pas cette information les coordonnées 0 0 sont retournées. 
> **MOGWAI CLI** gère cette fonction.
***

### `console.foreground`
Demande au host de changer la couleur d’affichage des caractères en passant en paramètre le nom de la couleur à utiliser.

Les couleurs définies dans **MOGWAI CLI** sont :
- `'BLACK'`
- `'WHITE'`
- `'BLUE'`
- `'RED'`
- `'YELLOW'`
- `'GREEN'`

```
'RED' console.foreground
```
***

### `console.background`
Demande au host de changer la couleur de fond de l’écran.
> Dans **MOGWAI CLI**, utilise les mêmes couleurs que pour `console.foreground`.
***

### `console.inputkey`
Demande au host de fournir le code de la touche actuellement tapée.
***

### `http.get`
Effectue un get http sur une uri en stipulant les valeurs des headers nécessaires. 

Les paramètres sont passés par un record :

```
[
    uri: "https://api.github.com/orgs/dotnet/repos" 
    requestHeaders: [User-Agent: ".NET Foundation Repository Reporter" token: "XXXXX"]
] http.get
```

La réponse est un record contenant les clés suivantes :

| Clé | Usage |
|-----|-------|
| `state:` | `true` si tout s'est bien passé. |
| `statusCode:` | Le status code effectivement retourné (ex 200). |
| `response:` | Un data contenant la réponse.<br>Si une erreur survient cette clé n'est pas présente dans la réponse.|

### `http.post`
Effectue un post http sur une uri en stipulant les request headers, les content headers et le content.

Tous les paramètres sont définis dans un record passé en paramètre :

```
[
    uri: "https://api.github.com/orgs/dotnet/repos" 
    requestHeaders: [ ]
    contentHeaders: [ ]
    content: DATA
]
```

Le content est de type data.

La réponse, un record, est formatée exactement comme celle de la fonction `http.get`
***

### `->uri`
Compose une uri à partir d'un record dont les clés correspondent aux différentes parties d'une uri :

```
[
    url: "https://www.google.com" 
    path: "api/v0/login" 
    query: [id: "50" name: "TOTO"]
] ->uri 

# Pose sur la pile "https://www.google.com:443/api/v0/login?id=50&name=TOTO"
```
***

### `->urlEncode`
Encode une chaîne d’URL passée en paramètre. 

Cette fonction peut être utilisée pour encoder l’URL entière, y compris les valeurs de chaîne de requête. 
L’encodage d’URL convertit les caractères qui ne sont pas autorisés dans une URL en équivalents caractère-entité. 
Par exemple, lorsque les caractères < et > sont incorporés dans un bloc de texte à transmettre dans une URL, ils sont encodés en %3c et %3e.
***

### `process.start`
Lance un processus. 

Les informations concernant le processus sont fournies via un record composé des clés suivantes :

| Clé | Usage |
|-----|-------|
| `filename:` | Fichier à exécuter (ex notepad.exe)|
| `arguments:` | Arguments à utiliser pour lancer le processus.|
| `workingDirectory:` | Permet de définir le dossier courant du processus.|
| `wait:` | Si `true` attend la fin de l'exécution du processus pour rendre la main.|

> Seule la clé `filename:` est obligatoire.

```
[
    filename: "toto.exe" 
    arguments: "/u -K" 
    workingDirectory: "C:\...." 
    wait: true ] process.start
```
***

## LES FONCTIONS DE DEBOGGAGE (utilisées avec MOGWAI STUDIO)

### `debug.write`
Demande au host et à **MOGWAI STUDIO** (si connecté) d'afficher un message dans la console de debug.

```
"Message de debug" debug.write
```
***

### `debug.clear`
Demande au host et à **MOGWAI STUDIO** (si connecté) d'effacer l'écran de debug.
***

### 'debug.run'
Lance un programme en mode debug. Le nom du programme est une chaîne de caractères passée en paramètre.
***

### `debug.halt` ou `¤`
Effectue une pause. Correspond à un breakpoint.

Le programme doit être lancé en debug pour que le point d'arrêt soit pris en compte. 
Quand l'exécution arrive sur cette instruction, le runtime se met en pause.
Il est alors possible d'avancer pas à pas si nécessaire.

```
1 10 for 'i' do
{
    i ?
    100 wait
    
    # On pose un point d'arrêt ici
    debug.halt
}
```
***

### `debug.tron`
Active la trace. La durée entre chaque instruction est définie en paramètre en millisecondes. 
Si **MOGWAI STUDIO** est connecté il affiche en temps réel l'instruction en cours d'exécution.

```
250 debug.tron
```
***

### `debug.troff`
Désactive la trace.
***

## LES FONCTIONS DE GESTION DU TEMPS

### `now`
Retourne la date courante de votre machine sous la forme d'un nombre qui représente le nombre d'intervalles de 100 nanosecondes qui se sont écoulés depuis minuit, le 1er janvier 0001.

Par exemple, le nombre 6.389664359647076E+17 correspond à la date 21/10/2025 à 11:39:56
***

### `->date`
Convertit une date numérique en composants de date et heure.

Cette fonction retourne un record composé des clés suivantes :

| Clé | Usage |
|-----|-------|
| `day:` | Jour.|
| `month:` | Mois.|
| `year:` | Année.|
| `hour:` | Heures.|
| `minute:` | Minutes.|
| `second:` | Secondes.|
| `dayOfYear:` | Numéro du jour dans l'année.|
| `dayOfWeek:` | Numéro du jour dans la semaine.<br>(dimanche=0, lundi=1, …, samedi=6)|

```
now ->date

# Si on est le 02/10/2025 à 11:51:29
# Pose sur la pile [day: 21 month: 10 year: 2025 hour: 11 minute: 51 second: 29 dayOfYear: 294 dayOfWeek: 2]
```
***

### `->duration`
Retourne une durée sous la forme d'un record composé des clés suivantes :

| Clé | Usage |
|-----|-------|
| `days:` | Nombre de jours écoulés.|
| `hours:` | Nombre d'heures écoulées.|
| `minutes:` | Nombre de minutes écoulées.|
| `milliseconds:` | Nombre de millisecondes écoulées.|

Typiquement pour calculer le temps passé entre 2 moments, on peut mémoriser le `now` du début, puis à la fin soustraire le `now` courant à celui du début, puis utiliser la fonction `->duration` pour récupérer le temps écoulé entre ces 2 moments.

```
now 2500 wait now - abs ->duration

# Pour une durée totale de 2 secondes et 507 millisecondes
# Pose sur la pile [days: 0 hours: 0 minutes: 0 seconds: 2 milliseconds: 507]
```
***

### `->day`
Retourne le jour du mois d'une date numérique passée en paramètre.
***

### `->month`
Retourne le mois d'une date numérique passée en paramètre.
***

### `->year`
Retourne l'année d'une date numérique passée en paramètre.
***

### `->hour`
Retourne l'heure d'une date numérique passée en paramètre.
***

### `->minute`
Retourne les minutes d'une date numérique passée en paramètre.
***

### `->second`
Retourne les secondes d'une date numérique passée en paramètre.
***

### `->millisecond`
Retourne les millisecondes d'une date numérique passée en paramètre.
***

### `->days`
Retourne une durée sous la forme d'un nombre de jours.
***

### `->hours`
Retourne une durée sous la forme d'un nombre d'heures.
****

### `->minutes`
Retourne une durée sous la forme d'un nombre de minutes.
***

### `->seconds`
Retourne une durée sous la forme d'un nombre de secondes.
***

### `->milliseconds`
Retourne une durée sous la forme d'un nombre de millisecondes.
***
 
## LES FONCTIONS DE GESTION DES TACHES

### `defTask`
Permet de créer une nouvelle tâche. Les paramètres de création sont fournis au moyen d'un record.
***

### `task.wait`
Lance la tâche fille dont le nom est passé en paramètre et attend la fin de son exécution pour rendre la main.
***

### `task.run`
Lance la tâche fille dont le nom est passé en paramètre en rendant la main immédiatement.
***

### `task.isCompleted`
Retourne true si la tâche fille dont le nom est passé en paramètre est terminée.
***

### `task.isRunning`
Retourne true si la tâche fille dont le nom est passé en paramètre est en cours d'exécution.
***

### `task.purge`
Supprime la tâche dont le nom est passé en paramètre.
***

### `task.list`
Retourne la liste des noms de toutes les tâches filles existantes.
***

### `task.setResult`
Permet à une tâche fille de stocker son résultat. Cette fonction ne peut être utilisée que depuis le code d'une tâche fille. Le résultat peut être de n'importe quel type géré par **MOGWAI**.

```
"MonResultat" task.setResult
54 task.setResult
```
***

### `task.result`
Retourne le résultat de la tâche fille dont le nom est passé en paramètre. Par défaut le résultat a pour valeur `null`.
***

### `task.input`
Retourne la valeur fournie en entrée à la tâche fille lors de sa création. 
Peut-être de n'importe quel type géré par **MOGWAI**. 
> Cette fonction ne peut être utilisée que depuis le code d'une tâche fille.
***

### `task.name`
Retourne le nom de la tâche fille. 
> Cette fonction ne peut être utilisée que depuis le code d'une tâche fille.
***

### `task.join`
Attend que toutes les tâches listées en paramètre se terminent pour rendre la main.

```
('T1' 'T2' 'T3') task.join
```
***

### task.send
Envoi une valeur à la tâche fille dont le nom est passé en paramètre. La valeur peut avoir n'importe quel type géré par **MOGWAI**.

```
'T1' "MaValeur" task.send

'T1' 5456 task.send
```
***

### `task.publish`
Permet à une tâche fille de publier (envoyer) une valeur à sa tâche mère. 
La valeur publiée peut être de n'importe quel type géré par **MOGWAI**.

> Cette fonction ne peut être utilisée que depuis le code d'une tâche fille. 

```
"MaValeur" task.publish

2345 task.publish
```
***

### `task.flag.isSet`
Retourne `true` si le flag d'une tâche fille est actif. 
On passe en paramètre le nom de la tâche fille et le nom du flag à tester.
***

### `task.flag.isClear`
Retourne `true` si le flag d'une tâche fille est inactif. 
On passe en paramètre le nom de la tâche fille et le nom du flag à tester.
***

### `task.flag.set`
Permet d'activer le flag d'une tâche fille. 
On passe en paramètre le nom de la tâche fille et le nom du flag à activer.
***

### `task.flag.clear`
Permet de désactiver le flag d'une tâche fille. 
On passe en paramètre le nom de la tâche fille et le nom du flag à désactiver.
***

## LES FONCTIONS DE GESTION DES PORTS SERIE

Sous Windows, MacOS et Linux, **MOGWAI** sait utiliser les ports série.

### `serial.open`
Ouvre un port de communication série. 

Les paramètres sont fournis dans un record :

| Clé | Usage |
|-----|-------|
| `name:` | Nom du port série (obligatoire).|
| `port:` | Port de communication à ouvrir (obligatoire).|
| `speed:` | Vitesse de communication en bauds (obligatoire)|
| `parity:` | Parité à utiliser. Valeurs possible :<br>"N" (none)<br>"E" (even)<br>"O" (odd)<br>"M" (mark)<br>"S" (space)<br>Valeur par défaut : "N".|
| `dataBits:` | Nombre de bits de données. Valeurs entre 5 et 8.<br>Valeur par défaut 1.|
| `stopBits:` | Nombre de bits de stop. Valeurs possibles : 0, 1, 1.5 et 2<br>Valeur par défaut 8.|
| `handshake:` | Contrôle de flux. Valeurs possibles :<br>"N" (none)<br>"X" (XON/XOFF)<br>"RTS" (rts/cts)<br>"RTSX" (rts/cts + XON/XOFF)<br>Valeur par défaut "N".|
| `newLine:` | Caractères correspondants à une nouvelle ligne.<br>Valeur par défaut "\r\n"|
| `dtr:` | Data Terminal Ready<br>Valeurs possibles true/false.<br>Valeur par défaut true.|
| `encoding:` | Format d'encodage des chaînes de caractères.<br>Valeurs possibles : "ASCII", "UTF8"<br>Valeur par défaut "ASCII".|
| `readBufferSize:` | Taille du buffer d'entrée.<br>Valeur par défaut 1024.|
| `writeBuffersize:` | Taille du buffer de sortie.<br>Valeur par défaut 1024.|
| `readTimeout:` | Durée en millisecondes avant un timeout en lecture.|
***

### `serial.close`
Ferme le port série dont le nom est passé en paramètre.
***

### ``serial.isOpen``
Retourne true si le port série dont le nom est passé en paramètre est ouvert.
***

### `serial.read`
Effectue une lecture de n octets depuis le port série dont le nom est passé en paramètre. 
Retourne un data composé des valeurs lues.

```
'P1' 50 serial.read

# Va lire 50 octets depuis 'P1' et poser le DATA correspondant sur la pile.
```
***

### `serial.readLine`
Lit une ligne depuis le port série dont le nom est passé en paramètre. 
La fonction retourne la chaîne de caractères (la ligne) lue.

```
'P1' serial.readLine
# Pose sur la pile le DATA correspondant aux informations lues.
```
***

### `serial.write`
Ecrit le contenu d'un DATA sur le port série dont le nom est passé en paramètre.

```
'P1' DATA:FF45AB23DE serial.write
```
***

### `serial.writeLine`
Ecrit une chaîne de caractères sur le port série dont le nom est passé en paramètre. 
La chaîne de caractères sera automatiquement complétée avec les caractères définis par `newLine:` pendant l'ouverture du port.

```
'P1' "Bonjour !" serial.writeLine
```
***

### `serial.available`
Retourne le nombre de bytes en attente dans le buffer d'entrée du port série dont le nom est passé en paramètre.
***

### `serial.clearInputBuffer`
Efface le buffer d'entrée du port série dont le nom est passé en paramètre.
***

### `serial.ports`
Retourne la liste des ports série connu du système.

```
serial.ports
# Pose sur la pile les ports connus de la machine. Ex ("COM1" "COM7" "COM12")
```
***

### `serial.info`
Retourne les informations du port série dont le nom est passé en paramètre. 
Les informations sont retournées sous la forme d'un RECORD composé des mêmes clés que celles utilisées pour ouvrir le port série (fonction serial.open).
> La clé `available:` est ajoutée avec le nombre de bytes en attente dans le buffer d'entrée.
***

### `serial.list`
Retourne la liste des ports série ouverts.
***

### `serial.waitAnswers`
Lance une lecture sur un port série en indiquant les réponses possibles attendues. 
Le format des réponses attendues est le même que celui de la fonction `like`. 

Les paramètres sont passés par un RECORD qui possède les clés suivantes :

| Clé | Usage |
|-----|-------|
| `port:` | Nom du port à utiliser.|
| `patterns:` | Liste des réponses attendues possibles.|
| `timeout:` | Timeout de lecture. Valeur par défaut 1000.|
| `sanitize:` | true si vous voulez que MOGWAI nettoie les réponses avant de les traiter.<br>Valeur par défaut true.|

Si vous attendez par exemple comme réponses possibles (plusieurs réponses qui se suivent) :

```
AT+BLE=1
AT+TEST=OK
OK

```
Vous pouvez utiliser comme pattern :

```
("AT+BLE=1" "AT+TEST=OK" "OK")
```

Si les réponses possibles peuvent varier légèrement on peut les prendre en compte.

Si par exemple on peut avoir 
```
AT+BLE=1
AT+TEST=OK
OK
```

Mais aussi parfois :

```
AT+BLE=0
AT+TEST=NO
OK
```

Et qu'on veut gérer les deux cas en une seule fois, on peut utiliser le pattern suivants :

```
("AT+BLE=1|AT+BLE=0" "AT+TEST=OK|AT+TEST=NO" "OK")
```

On peut aussi utiliser des jokers (voir fonction `like`) :

```
("AT+BLE=#" "AT+TEST=??" "OK")
```

La fonction retourne toutes les réponses reçues sous la forme d'une liste de chaînes de caractères.
 
## LES FONCTIONS DE GESTION DES EVENEMENTS

### `EVENT`
C’est la version 100% RPN de la fonction `event … do` édulcorée.

Pour répondre à un événement vous devez indiquer le nom de l’événement et le code à exécuter quand il est déclenché.

```
'MonEvenement' « "Bonjour !" ? » EVENT

# On peut aussi écrire la version édulcorée :

event 'MonEvenement' do « "Bonjour !" ? »
````
***

### `event.purge`
Supprime la prise en compte d’un événement dont le nom est passé en paramètre.
***

### `event.list`
Retourne la liste de tous les événements déclarés pris en charge.
***

### `event.fire`
Déclenche un événement en direction du runtime. 
On passe en paramètres le nom de l’événement, un objet qui accompagne l’événement et qui sera récupéré via la variable locale `eventData` dans le code de l’événement.

```
'MonEvenement' "Hello" event.fire
```
***

### `event.host.fire`
Déclenche un événement en direction du host. 
On passe en paramètres le nom de l’événement, un objet que le host récupèrera (`null` si rien à envoyer).

```
'MonEvenement' "Hello" event.host.fire
```
***

## LES FONCTIONS DE GESTION DE FICHIERS

### `file.pack`
Sérialise un objet passé en paramètre et le stocke dans un fichier.  
On passe en paramètre l'objet à sérialiser et le nom du fichier à utiliser.

```
(1 2 3 4) "MonFichier" file.pack
# Va sérialiser la liste dans le fichier "MonFichier".
```
***

### `file.unpack`
Désérialise un objet depuis un fichier. 
On passe en paramètre le nom du fichier dans lequel se trouve l'objet. 

Si on part de l'exemple de la fonction `file.pack` :

```
"MonFichier" file.unpack va retourner la liste (1 2 3 4)
```
***

### `file.read`
Lit le contenu entier d'un fichier et le retourne sous la forme d'un data. 
On passe en paramètre le nom du fichier à lire.
***

### `file.write`
Ecrit dans un fichier dont on passe le nom en paramètre le contenu d'un DATA.

```
DATA:FF45ABEA23 "MonFichier" file.write
# va écrire les octets 0xFF, 0x45, 0xAB, 0xEA et 0x23 dans le fichier.
```
***

### `file.purge`
Supprime un fichier dont on a passé le nom en paramètre.
***

### `dir.directories`
Retourne la liste de tous les dossiers présents dans le dossier courant.
***

### `dir.files`
Retourne la liste de tous les fichiers contenus dans le dossier courant.
***

### `dir.programs`
Retourne la liste de tous les programmes (fichier avec l'extension .mog) contenus dans le dossier courant.
***

### `dir.path`
Retourne le path courant sous la forme d'une liste de dossiers. 
Le chemin est décomposé en autant d'éléments que de dossiers. 

```
Le path "HOME\Dossier1\Dossier2" sera retourné sous la forme ("HOME" "Dossier1" "Dossier2")
```
***

### `dir.make`
Permet de créer un nouveau dossier dans le dossier courant.
On passe en paramètre le nom du nouveau dossier.
***

### `dir.purge`
Supprime le dossier dont le nom est passé en paramètre. 
Le dossier est dans le dossier courant.
***

### `dir.home`
Remonte au dossier HOME.
***

### `dir.enter`
Entre dans un dossier présent dans le dossier courant en passant en paramètre son nom.

```
"LeDossier" dir.enter
```
***

### `dir.rename`
Renomme un dossier (du dossier courant) en passant en paramètre le nom actuel et le nouveau nom.

```
"AncienNom" "NouveauNom" dir.rename
```
***
 
## LES FONCTIONS DE GESTION DE BASE DE DONNEES

### `db.create`
Crée un nouveau fichier SQLite dans le dossier courant qui contiendra toutes les tables de la base de données. 
On passe le nom du fichier en paramètre.

```
"MaBase.db" db.create
```
***

### `db.open`
Ouvre une base de données. 
On passe en paramètre le nom du fichier. 

Cette fonction retourne un identifiant unique sous la forme d'un nom qu'il faut mémoriser pour ensuite accéder aux fonctions pour cette base de données.

```
"MaBase.db" db.open -> '$DB' 
# Dans cet exemple, $DB pourrait contenir un nom du genre : 'DBC:CB3D468A010F423891CCBF62D477CB3A'
```
***

### `db.close`
Ferme une base de données, on passe son identifiant en paramètre.
***

### `db.execute`
Exécute une requête commande SQL (par exemple une commande de création de table) vers la base dont on passe le nom en paramètre. 
On peut passer des paramètres sous la forme d'un RECORD.

> Les paramètres peuvent seulement être du type `.number`, `.string` ou `.data`

```
$DB "Create Table..." db.execute

$DB "Create Table..." [p1: 50 p2: "ESSAI"] db.execute
```
***

### `db.executeScalar`
Exécute une requête scalaire. 
Elle a le même format que db.execute. 
Elle pose sur la pile son résultat.
***

### `db.query`
Exécute une requête d'interrogation sur la base de données dont passe le nom en paramètre.

```
$DB "SELECT * FROM MaTable" db.query
# Pose sur la pile une liste de records. Chaque record étant composé des champs de la table
```
***

### `db.purge`
Supprime une base de données dont on donne le nom de fichier en paramètre.
***

### `db.list`
Retourne la liste de toutes les bases de données ouvertes.
***

### `db.transaction.begin`
Démarre une transaction.
***

### `db.transaction.commit`
Valide une transaction.
***

### `db.transaction.rollback`
Abandonne une transaction.
***

### `db.info`
Retourne des informations sous la forme d'un record sur la base de données dont on passe le nom en paramètre.

Le record possède les clés suivantes :

| Clé  | Usage |
|------|-------|
| `name:` | Le nom de la base de données.|
| `database:` | Le fichier contenant la base de données.|
| `transaction:` | `true` / `false`|
***

## LES FONCTIONS DE GESTION DES TIMERS

### `EVERY`
C’est la version 100% RPN de la fonction `timer … every … do` édulcorée.

Crée un timer de type `every`.

Les paramètres sont :
- Le nom du timer.
- L’intervalle de répétition (en millisecondes).
- Le code à exécuter à chaque itération.

```
'MonTimer' 1000 « "Bonjour !" ? » EVERY
```
***

### `AFTER`
C’est la fonction 100% RPN de la version `timer … after … do` édulcorée.

Crée un timer de type `after`. Les paramètres sont les mêmes que ceux de la fonction `EVERY`.

```
'MonTimer' 1000 « "Bonjour !" ? » AFTER
```
***

### `timer.start`
Lance le timer dont le nom est donné en paramètre.
***

### `timer.stop`
Stoppe le timer dont le nom est donné en paramètre.
***

### `timer.purge`
Supprime le timer dont le nom est donné en paramètre.
***

#### `timer.state`
Retourne `true` si le timer est lancé.
***

### `timer.list`
Retourne la liste de tous les timers déclarés quel que soit leur statut (lancé ou stoppé).
***

### `DI`
Suspend le déclenchement de tous les timers et de tous les événements. 
> Attention, ils sont mis en attente et seront exécutés quand les interruptions seront à nouveau activées.
***

### `EI`
Autorise le déclenchement des timers et des événements.
***

