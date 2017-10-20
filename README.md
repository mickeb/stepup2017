# Step up 2017 - Elixir handson

## Installation/Förberedelse ##

Dessa steg gäller för Windows, Linux och MacOS.

1. Ladda hem och installera version 5.1 av virtualbox (<https://www.virtualbox.org/wiki/Download_Old_Builds_5_1>)

2. Ladda hem och installera senaste versionen av vagrant (<https://www.vagrantup.com/downloads.html>)

3. Klona ner detta repository.

4. Ståendes i rotkatalogen av repositoriet kör `vagrant up`. Kommandot kan ibland ta ett tag då OS "imagen" ska laddas ner och nätverket m.m skall konfigureras.

5. Kör `vagrant ssh` för att logga in labbmiljön (Windows-användare behöver eventuellt installera en ssh klient separat. Tips: [tortoisegit](https://tortoisegit.org/) bundlar en klient i sin installation).

6. Kör `elixir -v`. Förväntad output är: *Elixir 1.5.2*

7. Kör `exit` för att avsluta SSH-sessionen

8. Kör `vagrant halt` för att avsluta utvecklingsmiljön
