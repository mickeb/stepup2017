# Elixir hand-one lab

## REPL

Elixir kommer med en s.k REPL (Read–eval–print loop). Ett utmärkt verktyg att använda när man lär sig språket eller snabbt vill testa en bit kod.

REPL startas med följande kommando: `iex`

### En riktigt klassiker

Väl inne i Elixirs REPL kan vi börja skriva vår första Elixir-kod. Varför inte börja med en riktigt klassiker?

```elixir
IO.puts("Hello world")
```

Bra jobbat!

Vi tar det ett steg längre. Låt oss definiera en variabel som representerar meddelandet.

```elixir
hello_msg = "Hello world"
IO.puts(hello_msg)
```

## Hjälpfunktioner

### Help? ..anyone?

I Elixir grupperas namngivna funktioner i namngivna moduler. I modulen `String` hittas t.ex. funktioner för stränghantering.

I "Hello world"-exemplet anropade vi funktionen `puts` i modulen `IO` (oroa dig inte, vi kommer snart tala mer om moduler och funktioner).

Visst skulle det vara trevligt att kunna få fram dokumentation om både modul och funktion?

I Elixirs REPL (från och med nu kallad `iex`) finns hjälpen lyckligtvis inte långt borta.

```elixir
h(IO)
h(IO.puts/1)
```

Nu undrar du kanske vad `/1` är för något. I Elixir används detta syntax för att tala om funktionens aritet, d.v.s hur många argument en funktionen förväntar sig.

I `iex` kan du även få information om datastrukturer.

```elixir
i("Hello world")
```

Ett snabbt sätt att lista alla funktioner en modul innehåller är att använda s.k. auto-completion.

Ange t.ex. `String.` följt av ett tryck på TAB-tangenten.

## Gällande hjälp om hjälpen

Både `h` och `i` finns defierade i modulen `IEx.Helpers`. Det rekommenderas starkt att läsa dokumentationen för denna modul om det känns som Elixir kan vara något som är värt att lägga ner tid på.

```elixir
h(IEx.Helpers)
```

## Filändelser

Elixirkod kan köras i script-läge. Detta fungerar på samma vis som i python eller ruby.

`elixir <myscript>`

Koden i det angivna scriptet kompileras och körs i ett enda svep.

Det finns därför två filändelser, `.ex` samt `.exs`.

`.ex` markerar att filen behöver kompileras till körbarkod i ett separat steg. Vi kommer dock endast använda oss av `.exs` vilket markerar filen som ett direkt körbart script.

### Ett första script

Skapa filen `my_elixir_script.exs`. Öppna den sedan i din favoriteditor och skriv in:

```elixir
IO.puts("Hello world")
```

Kör sedan scriptet:

`elixir my_elixir_script.exs`

Om du ser *Hello world* skrivas ut är det helt ok att skryta lite för grannen om dina Elixir-framgångar.

### Konsten att kommentera

I Elixir räknas alla rader som börjar på `#` som kommentarer.

## Moduler och funktioner

**Viktigt**

Från och med nu rekomenderas att skriva och exekvera kod i script-läge. Du har författarens tillåtelse att återanvända `my_elixir_script.exs` om så önskas.

### Moduler

I Elixir måste alla namngivna funktioner definieras in en namngiven modul.

En modul definieras på detta vis.

```elixir
defmodule MyModule do
end
```

Moduler kan med lätthet nästlas.

```elixir
defmodule MyModule do
  defmodule MyInnerModule do
  end
end
```

Alternativt

```elixir
defmodule MyModule.MyInnerModule do
end
```

### Funktioner

Ett funktionellt språk utan funktioner är inte mycket att hänga i julgranen. Låt oss därför definiera vår första funktion och anropa den.

```elixir
defmodule MyModule do
  def hello_world do
    IO.puts("Hello world")
  end
end

MyModule.hello_world()
```

Argument definieras utan överraskningar.

```elixir
defmodule MyModule do
  def greet(greeting) do
    IO.puts(greeting)
  end
end

MyModule.greet("Hello world")
```

Multipla argument separeras med `,`

```elixir
defmodule MyModule do
  def greet(greeting, compiler_complains_about_me) do
    IO.puts(greeting)
  end
end

MyModule.greet("Hello world", "Not used")
```

En funktion i Elixir returnerar resultatet av det sista evaluerade uttrycket.

```elixir
defmodule MyModule do
  def return_hello_world do
    "Hello world"
  end
end

IO.puts MyModule.return_hello_world()
```

## Pipes (allt annat än rörigt, trots namnet)

Elixir implementerar något som kallar för "pipes". Har du arbetet i ett unix-skal som t.ex. "bash" kommer du genast känna igen konceptet.

Problemet pipes löser är följande.

Du vill skriva en web-scraper som hämtar hem de senaste nyheterna från https://elixir-lang.org. Detta skulle kunna brytas ner i tre steg

1. Hämta rådata (markup) via ett http request.
2. Extrahera nyheter ur rådata.
3. Spara extraherade nyheter i databas.

Utan att använda pipes skulle vi skriva något i form med

```
raw_data = get_raw_data()
news = parse_raw_data(raw_data)

save_news(news)
```

Eller kanske t.o.m.

```
save_news(parse_raw_data(get_raw_data()))
```

Med pipes kan detta beskrivas som ett mer lättläst flöde.

```elixir
get_raw_data() |> parse_raw_data() |> save_news()
```

Föregående funktions returvärde "pipas" in som nästkommandes funktions första parameter. Gesvint!

Värt att nämna är att vid längre pipelines används ofta följande formattering

```elixir
get_raw_data()
|> parse_raw_data()
|> save_news()
```

## Övningsdags (1)

Skriv en metod som tar emot två strängar, konkatenerar dem, konvertar samtliga tecken till versaler och skriver ut resultatet. Glöm inte att använda pipes!

Exempel

```elixir
MyModule.my_function("foo", "bar")
"FOOBAR"
```

*Tips:* Läs dokumentation för `Kernel.<>`

## Mer funktioner!

### Anonyma funktioner

Förutom namngivna funktioner, som vi hittils arbetat med, stödjer Elixir anonyma funktioner.

I exemplet nedan definierar vi en anonym funktion och passar även på att binda funktionen till en variabel.

```elixir
my_function = fn() -> "Hello world" end
```

När det kommer till att anropa anonyma funktioner bjuder Elixir på en liten överraskning i form av syntaxen som används.

```elixir
say = fn(message) -> IO.puts(message) end
say.("Hello World")
```

`.` tecknet används för att tydligt markera att det är anonym funktion och inget annat som anropas.

### Högre ordningens funktioner

Anonyma funktioner i elixir kan användas som både funktionsparametrar och returvärden.

Här följer ett exempel på en funktion som tar en anomym funktion som argument för sedan anropa denna.

```elixir
defmodule MyModule do
  def message_printer(func) do
    func.() |> IO.puts()
  end
end

calculate_hello_message = fn() -> "Hello world" end

MyModule.message_printer(calculate_hello_message)
```

## Övningsdags (2)

I exemplet ovan skickas en anonym funktion som parameter. I denna övning skriva en funktion som returnerar en anonym funktion.

Skriva en konfigurerbar adderare. Det finns ingen anledning oroa sig, det hela är relativt okomplicerat. Se exemplet nedan.

```elixir
my_two_adder = Adder.create_adder(2)
my_two_adder.(2) |> IO.puts()
# 4

my_five_adder = Adder.create_adder(5)
my_five_adder.(2) |> IO.puts()
# 7
```

**Tips**

Elixir stödjer vad som kallas för "closures". Detta innebär att anonyma funktioner kan referera till variabler och funktionsparametrar i scopet där de definieras. Scopet i detta fall är adderar-funktionen.

## Pattern matching

I Elixir är s.k. "pattern matching" ett centralt och väldigt kraftfullt koncept. Vi kommer här endast skrapa på ytan.

### Innan vi börjar matcha alla dessa mönster

Vi har hitills använt oss av Strängar och Heltal. För denna sektion kommer vi introducera datatyperna Map och Atom.

Map kan i andra språk kallas för "Hash", "Dict" eller "Associative array".

Datatypen låter oss associera till data via nycklar.

```elixir
my_map = %{ "first_name" => "Jose", "last_name" => "Valim" }

my_map["first_name"]
# Jose

my_map["last_name"]
# Valim
```

Datatypen Atom värde definieras av sitt namn. Detta bekrivs bäst med ett kodexempel där vi jämnför två värden av typen Atom med Elixirs likhetsoperator `==`

```elixir
:foo == :foo
true

:foo == :bar
false
```

Dessa två datatyper används ofta ihop.

```elixir
my_map = %{:first_name => "Jose", :last_name => "Valim"}

my_map[:first_name]
# Jose

my_map[:last_name]
# Valim
```

Utöver att användas som nyckel används datatypen Atom väldigt ofta i funktioners returnvärden. Detta något vi kommer titta närmare på härnäst.

### En första matchning

Hoppa in i `iex` och anropa följande.

```elixir
IO.puts("Pattern matching here i come!")
```

Noterade du returvärdet? Just det, ett värde av typen vi nu vet kallas Atom. Låt oss utföra en matchning på värdet med hjälp av operatorn `=`.

```elixir
:ok = IO.puts("Pattern matching here i come!")
```

Pröva att föröka matcha på något annat än `:ok`.

Redan tillbaka? Det gick inte så bra, eller hur?

Vad som går bra däremot är följande.

```elixir
return_status = IO.puts("Pattern matching here i come!")
```

Här binds matchat värde till en variabel. I det här fallet ett värde utan restriktioner.

Känns det konstigt? Vi tar ett exempel till. Denna gång vår andra nyfunna vän Map

```elixir
%{:first_name => "Jose", :last_name => "Valim" } = %{:first_name => "Jose", :last_name => "Valim"}
```

Japp, matchar.

```elixir
%{:first_name => "Jose", :last_name => "Valim"} = %{:first_name => "Jose", :last_name => "Not Valim"}
```

Nopp, matchar inte.

```elixir
%{:first_name => "Jose", :last_name => last_name} = %{:first_name => "Jose", :last_name => "Not Valim"}
```

Match! Vi har nu matchat värdet av `:first_name` mot "Jose" och samtidigt bundit matchningen av `:last_name` till variabel `last_name`.

## Mera match!

Elixir möjliggör pattern matching på funktionargument. Skriv in detta i din favorit `.exs` fil och kör scriptet.

```elixir
defmodule MyModule do
  def say("Hello") do
    IO.puts "Someone is trying to say hello!"
  end
end

MyModule.say("Hello")
```

Funktionen körs som förväntat.

Uppdatera sedan funktionsargumentet i funktionsanropet till t.ex "Goodbye" och kör scriptet igen.

```elixir
defmodule MyModule do
  def say("Hello") do
    IO.puts "Someone is trying to say hello!"
  end
end

MyModule.say("Goodbye")
```

Nu gick det sämre, eller hur? Elixir kan inte matcha argumentet och talar också om det för oss.

Nu är det så fiffigt att Elixir låter oss definiera samma namngivna funktion flera gånger med olika matchningar på funktionsargumenten.

```elixir
defmodule MyModule do
  def say("Hello") do
    IO.puts "Someone is trying to say hello!"
  end

  def say("Goodbye") do
    IO.puts "Someone is trying to say goodbye!"
  end
end

MyModule.say("Hello")
MyModule.say("Goodbye")
```

Vi kan även binda matchningar till funktionsargument.

```elixir
defmodule MyModule do
  def say(%{:hello => name}) do
    IO.puts "Someone is trying to say hello to #{name}!"
  end
end

MyModule.say(%{:hello => "Eric"})
```

Voila!


## Övningsdags (3)

Skriv en funktion som tackar namngiven person för dennes arbete med Elixir. Ämnas tacket José Valim sker det på hans modersmål, portugisiska, istället för engelska.

Namnet på personen ska kunna anges som Map eller sträng.

```elixir
Thanks.say("Aleksei Magusev")
Thanks.say(%{:first_name => "Aleksei", :last_name => "Magusev"})

# -> Thank you Aleksei Magusev for all your work on Elixir!

Thanks.say("José Valim")
Thanks.say(%{:first_name => "José", :last_name => "Valim"})

# -> Obrigado José Valim por todo seu trabalho em Elixir!
```

## Ämnesförslag för fortsatt labbande

### Datatyper

Vi har under denna lab endast använt ett par av de datatyper som Elixir erbjuder. Elixirs officiella hemsida är en mycket bra källa för att lära sig om resterande.

https://elixir-lang.org/getting-started/basic-types.html

### Kontrollstrukturer

Elixir erbjuder en mängd kontrollstrukurer utöver välkända "if/else".

Ett utav dem är "case".

https://elixir-lang.org/crash-course.html#case
https://elixirschool.com/en/lessons/basics/control-structures/#case

### Guards

Ett mycket kraftfullt koncept ihop med pattern matching.

https://hexdocs.pm/elixir/master/guards.html
