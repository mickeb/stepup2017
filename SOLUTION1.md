```elixir
defmodule MyModule do
  def my_function(s1, s2) do
    (s1 <> s2)
    |> String.upcase
    |> IO.puts
  end
end
```
