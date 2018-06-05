# Translation Guide

> In this guide we will explain how to translate the game into any language, from the beggining. In order to do that, you should follow these steps.

### Steps:

1) First, in every *.cpp* file that have strings to be translated, include at the beggining of the file these libraries:

```sh
#include <stdio.h>
#include <stdlib.h>
#include <libintl.h>
#include <locale.h>
#define _(STRING) gettext(STRING)
```

Then, into every `void` or `function` that have those strings to be translated, we need to include these three lines:

```sh
setlocale (LC_ALL, "");
bindtextdomain ("nsnake", "/usr/share/locale/");
textdomain ("nsnake");
```

Where `"nsnake"` is name for the file that contains the translated strings and `"/usr/share/locale/"` is where translations are stored locally.

Finally, we will enclose each string into these structure '_("String")', for eg.: `_("Main Menu")`.

2) You will create a folder into *'/translate/po/'* for each language you want to translate the game to. For instance, if you want to translate the game to Spanish, you will create the *es_AR* folder (this will contain the *.po* and *.mo* files)

3) Now, we will generate the template (a *.pot* file), that will contain the lines to be translated. We will overwrite these template, for each file that will contains lines for translate. 
For doing that, from the terminal you will run this command:

```sh
xgettext --keyword=_ --language=C++ --add-comments --sort-output -o translate/nsnake.pot <path-to-cppFile>/<example>.cpp
```

4) Then, we will generate the file containing all the translations (a *.po* file). We will modify these *.po* file, every time that we regenerate the previous template.
For the first time, you will run these command:

```sh
msginit -l es_AR.UTF-8 -o translate/es_AR/nsnake.po -i translate/nsnake.pot
```
If you are updating the .po file, just copy the new lines previously generated in the *.pot* file.

5) After creating or updating the *.po* file, you will generate the *.mo* file, with this command:

```sh
msgfmt --output-file=translate/es_AR/nsnake.mo translate/es_AR/nsnake.po
```

6) After that, you will have to move this *.mo* file to the appropiate language folder. For doing that, we will modify the Makefile, including two commands: one for moving this file when installing the game, and another one for deleting it when uninstalling.
For this purpose, insert these two lines:

* For installing it:
```sh
# Spanish (Argentina) translation
$(MUTE)-cp -v translate/po/es_AR/nsnake.mo /usr/share/locale/es_AR/LC_MESSAGES/
```

* For uninstalling it:
```sh
$(MUTE)-rm -f /usr/share/locale/es_AR/LC_MESSAGES/nsnake.mo
```

Take into account the '-' symbol preceding the complete command. That is for not stopping the execution of the program if some of the languages folders do not exist in the S.O.

7) Finally, remember to recompile all your code before running it. For doing that, run this commands:

```sh
make
sudo make install
nsnake
```

**Thanks for reading ! ! !**