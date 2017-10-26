```elixir
defmodule Adder do
  def create_adder(value_to_add) do
    fn(input_value) -> input_value + value_to_add end
  end
end
```
