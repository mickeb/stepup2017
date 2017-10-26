```elixir
defmodule Thanks do
  def say(%{:first_name => first_name, :last_name => last_name}) do
    say("#{first_name} #{last_name}")
  end

  def say("José Valim") do
    IO.puts("Obrigado José Valim por todo seu trabalho em Elixir!")
  end

  def say(name) do
    IO.puts "Thank you #{name} for all your work on Elixir!"
  end
end
``
