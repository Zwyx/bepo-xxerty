# Bépo-Xxerty

Use Bépo on Linux while keeping Azerty or Qwerty shortcuts

*As Bépo is mainly used by French people, the rest of this file is written in French.*


## Présentation

Bépo-Xxerty permet d'utiliser la disposition de clavier Bépo tout en conservant les raccourcis clavier à leurs places. Ce comportement est difficile à mettre en place sous Linux ; l'astuce ici est de rendre la disposition **Bépo accessible avec la touche CapsLock** (Verrouillage majuscule).

Les premiers niveaux de caractères de Bépo-Xxerty sont ceux d'Azerty/Qwerty, et la touche CapsLock donne accès aux niveaux supérieurs : ceux de Bépo. Lors de l'utilisation des touches Control et/ou Alt, ce sont toujours les caractères Azerty/Qwerty qui sont utilisés. C'est ainsi que l'on peut utiliser les raccourcis Azerty/Qwerty (Ctrl+C, etc.) tout en tapant en Bépo.

Il est facile de désactiver l'indicateur lumineux de CapsLock pour ne pas qu'il soit toujours allumé pendant l'utilisation de Bépo.

Bépo-Xxerty fonctionne sous Linux avec X ; aucun test n'a été effectué avec Wayland.


## Mise en œuvre

Les fichiers de configuration du clavier se trouvent dans `/usr/share/X11/xkb/`. Ces fichiers étant susceptibles de changer d'une distribution à l'autre, ce repository ne procure pas ces fichiers complets ; seules les modifications à effectuer sont présentes.

1. Clonez ou téléchargez le repository Bépo-Xxerty. Pour la suite, `<bepo-xxerty>` représente le chemin d'accès à votre copie du repository.

2. Placez-vous dans `/usr/share/X11/xkb/types/` et créez une copie de sauvegarde de `complete` :
```
cd /usr/share/X11/xkb/types/
sudo cp complete complete.back
```

3. Placez le fichier `<bepo-xxerty>/src/types/bepo_xxerty` dans `/usr/share/X11/xkb/types/` :
```
sudo cp <bepo-xxerty>/src/types/bepo_xxerty /usr/share/X11/xkb/types/
```

4. Ouvrez le fichier `/usr/share/X11/xkb/types/complete` dans un éditeur de text (n'oubliez pas `sudo`), puis ajoutez-y la ligne `include "bepo_xxerty"` :
```
default xkb_types "complete" {
    include "basic"
    include "mousekeys"
    include "pc"
    include "iso9995"
    include "level5"
    include "extra"
    include "numpad"
    include "bepo_xxerty"
};
```

5. Placez-vous dans `/usr/share/X11/xkb/symbols/` et créez une copie de sauvegarde de `fr` :
```
cd /usr/share/X11/xkb/symbols/
sudo cp fr fr.back
```

6. Ouvrez le fichier `/usr/share/X11/xkb/symbols/fr` dans un éditeur de text (n'oubliez pas `sudo`), puis ajoutez-y le contenu de `<bepo-xxerty>/src/symbols/to-be-added-in-fr` à la suite de la définition Bépo standard. **Ne remplacez pas tout le contenu du fichier, ne faites qu'un ajout.**

7. Placez-vous dans `/usr/share/X11/xkb/rules/` et créez une copie de sauvegarde de `evdev.xml` :
```
cd /usr/share/X11/xkb/rules/
sudo cp evdev.xml evdev.xml.back
```

8. Ouvrez le fichier `/usr/share/X11/xkb/rules/evdev.xml` dans un éditeur de text (n'oubliez pas `sudo`), puis ajoutez-y le contenu de `<bepo-xxerty>/src/rules/to-be-added-in-evdev.xml` à la suite du bloc correspondant au Bépo standard. **Ne remplacez pas tout le contenu du fichier, ne faites qu'un ajout.**

Ce bloc :
```
        <variant>
          <configItem>
            <name>bepo</name>
            <description>French (Bepo, ergonomic, Dvorak way)</description>
          </configItem>
        </variant>
```
devient :
```
        <variant>
          <configItem>
            <name>bepo</name>
            <description>French (Bepo, ergonomic, Dvorak way)</description>
          </configItem>
        </variant>
        <variant>
          <configItem>
            <name>bepo-azerty</name>
            <description>French (Bepo-Azerty, ergonomic, Dvorak way, Active on Caps Lock)</description>
          </configItem>
        </variant>
        <variant>
          <configItem>
            <name>bepo-qwerty</name>
            <description>French (Bepo-Qwerty, ergonomic, Dvorak way, Active on Caps Lock)</description>
          </configItem>
        </variant>
```

Effectuez les étapes 9 et 10 si vous désirez désactiver l'indicateur lumineux de CapsLock pour ne pas qu'il soit toujours allumé pendant l'utilisation de Bépo.

9. Placez-vous dans `/usr/share/X11/xkb/compat/` et créez une copie de sauvegarde de `ledcaps` :
```
cd /usr/share/X11/xkb/compat/
sudo cp ledcaps ledcaps.back
```

10. Ouvrez le fichier `/usr/share/X11/xkb/compat/ledcaps` dans un éditeur de text (n'oubliez pas `sudo`), puis, dans le group `xkb_compatibility "caps_lock"`, modifiez la ligne `whichModState= Locked;` en `whichModState= None;`, et désactivez la ligne `modifiers= Lock;` : `// modifiers= Lock;`. Le résultat devrait être semblable à ceci :
```
default partial xkb_compatibility "caps_lock" {
    indicator "Caps Lock" {
        !allowExplicit;
        whichModState= None;
        // modifiers= Lock;
    };
};
```

11. L'installation est terminée. Pour l'activer temporairement, entrez :
```
setxkbmap fr bepo-azerty
```
ou
```
setxkbmap fr bepo-qwerty
```
en fonction de votre clavier physique.

Si tout fonctionne correctement et que vous souhaitez rendre Bepo-Azerty/Bepo-Qwerty disponibles dès le démarrage de votre ordinateur, rendez-vous dans la configuration du clavier de votre distribution Linux pour l'ajouter à la liste des dispositions actives (la première de la liste est celle activée lors du démarrage de l'ordinateur).


## Utilisation

Lorsque Bepo-Azerty/Bepo-Qwerty est active et que CapsLock est verrouillée, utilisez Bépo comme une disposition Bépo normale : Shift donne accès aux majuscules, AltGr au troisième niveau, et AltGr+Shift au quatrième.


## Particularité de Bépo-Qwerty

Les utilisateurs de Bépo sur un clavier **Qwerty** vont être confrontés à un étrange comportement dans certaines applications (XFCE terminal et Sublime Text, par exemple ; mais ni VSCode, ni Chrome) : l'impossibilité de saisir un A minuscule ! Presser la touche A produit le même résultat que presser Shift+A : un A majuscule !

Ceci vient indirectement du fait que le A de Qwerty et le A de Bépo sont à la même place...

La solution est simple : déplacer le A de Qwerty ou le A de Bépo. Exemples :
- Inverser le A et le Q de Qwerty.
- Inverser le A et le S de Qwerty.
- Inverser le A et le U de Bépo.
- Inverser le A de Qwerty avec n'importe quel autre caractère.
- Si, les seules fois que vous avez utilisé Ctrl+Q, c'était par mégarde, et que cela vous a fermé l'application dans laquelle vous étiez en train de travailler ($#@?#@$!!), alors autant supprimer complètement le Q du Qwerty. Mettez à sa place un Y par exemple, vous donnant ainsi un accès facile à la fonction « Annuler l'annulation ». (Si vous inversez Q et Y, vous allez probablement faire Ctrl+Q en voulant faire Ctrl+Y par automatisme... autant retirer complètement le Q et avoir deux Y sur son clavier.)

La solution fournit dans le fichier `<bepo-xxerty>/src/symbols/to-be-added-in-fr` est celle inversant le A et le S de Qwerty. Modifiez ce fichier si vous optez pour une autre solution.


## Personnaliser ses raccourcis clavier

Vous l'aurez compris en lisant le paragraphe précédant, Bépo-Xxerty vous donne la possibilité de personnaliser facilement vos raccourcis clavier sans modifier la disposition utilisée pour taper.
