# Advent of code 2023 Day 2

```elixir
Mix.install([
  {:kino, "~> 0.11.3"}
])
```

## Data

```elixir
field = Kino.Input.textarea("Input")
```

```elixir
input = Kino.Input.read(field)

data =
  input
  |> String.split("\n")
  |> Enum.map(fn x ->
    [game, pieces] = String.split(x, ":")

    [_, game] = String.split(game, " ")
    game

    ins =
      String.split(pieces, ";")
      |> Enum.map(fn y ->
        columns =
          String.split(y, ",")
          |> Enum.map(fn z -> String.trim(z) end)
          |> Enum.map(fn z ->
            [count, color] = String.split(z, " ")
            %{color: color, count: String.to_integer(count)}
          end)

        columns
      end)

    %{
      game: String.to_integer(game),
      ins: ins
    }
  end)

data
```

## Part 1

```elixir
# 12 red cubes, 13 green cubes, and 14 blue cubes

sumed =
  data
  |> Enum.map(fn x ->
    flat = List.flatten(x.ins)

    reds =
      Enum.filter(flat, fn y ->
        y.color == "red"
      end)
      |> Enum.map(fn y ->
        y.count
      end)
      |> Enum.max()

    greens =
      Enum.filter(flat, fn y ->
        y.color == "green"
      end)
      |> Enum.map(fn y ->
        y.count
      end)
      |> Enum.max()

    blues =
      Enum.filter(flat, fn y ->
        y.color == "blue"
      end)
      |> Enum.map(fn y ->
        y.count
      end)
      |> Enum.max()

    %{
      game: x.game,
      reds: reds,
      greens: greens,
      blues: blues,
      ins: x.ins
    }
  end)
  |> Enum.filter(fn x ->
    x.reds <= 12 and x.greens <= 13 and x.blues <= 14
  end)
  |> Enum.map(fn x -> x.game end)
  |> Enum.sum()
```

## Part 2

```elixir
# 12 red cubes, 13 green cubes, and 14 blue cubes

sumed =
  data
  |> Enum.map(fn x ->
    flat = List.flatten(x.ins)

    reds =
      Enum.filter(flat, fn y ->
        y.color == "red"
      end)
      |> Enum.map(fn y ->
        y.count
      end)
      |> Enum.max()

    greens =
      Enum.filter(flat, fn y ->
        y.color == "green"
      end)
      |> Enum.map(fn y ->
        y.count
      end)
      |> Enum.max()

    blues =
      Enum.filter(flat, fn y ->
        y.color == "blue"
      end)
      |> Enum.map(fn y ->
        y.count
      end)
      |> Enum.max()

    %{
      game: x.game,
      reds: reds,
      greens: greens,
      blues: blues,
      power: reds * greens * blues,
      ins: x.ins
    }
  end)
  |> Enum.map(fn x -> x.power end)
  |> Enum.sum()
```
