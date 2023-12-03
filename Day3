# Advent of code 2023 Day 3

```elixir
Mix.install([
  {:kino, "~> 0.11.3"}
])
```

## Data

```elixir
field = Kino.Input.file("data")
```

```elixir
value = Kino.Input.read(field)
path = Kino.Input.file_path(value.file_ref)
input = File.read!(path)

# input = """
# 467..114..
# ...*......
# ..35..633.
# ......#...
# 617*......
# .....+.58.
# ..592.....
# ......755.
# ...$.*....
# .664.598..
# """

data =
  input
  |> String.split("\n")
  |> Enum.with_index()
  |> Enum.map(fn {row, row_index} ->
    String.graphemes(row)
    |> Enum.with_index()
    |> Enum.map(fn {col, col_index} ->
      %{
        value: col,
        x: col_index,
        y: row_index
      }
    end)
  end)

data
```

## Part 1

```elixir
symbols =
  for row <- data,
      cell <- row,
      cell.value != "." and !Regex.match?(~r/^\d/, cell.value) do
    cell
  end

mapped =
  symbols
  |> Enum.map(fn s ->
    list = []

    list =
      case Enum.at(data, s.y - 1) do
        nil ->
          list

        adjacent_row ->
          list =
            case Enum.at(adjacent_row, s.x - 1) do
              number ->
                list ++ [number]

              nil ->
                list
            end

          list =
            case Enum.at(adjacent_row, s.x) do
              number ->
                list ++ [number]

              nil ->
                list
            end

          list =
            case Enum.at(adjacent_row, s.x + 1) do
              number ->
                list ++ [number]

              nil ->
                list
            end

          list
      end

    list =
      case Enum.at(data, s.y) do
        nil ->
          list

        adjacent_row ->
          list =
            case Enum.at(adjacent_row, s.x - 1) do
              number ->
                list ++ [number]

              nil ->
                list
            end

          list =
            case Enum.at(adjacent_row, s.x + 1) do
              number ->
                list ++ [number]

              nil ->
                list
            end

          list
      end

    list =
      case Enum.at(data, s.y + 1) do
        nil ->
          list

        adjacent_row ->
          list =
            case Enum.at(adjacent_row, s.x - 1) do
              number ->
                list ++ [number]

              nil ->
                list
            end

          list =
            case Enum.at(adjacent_row, s.x) do
              number ->
                list ++ [number]

              nil ->
                list
            end

          list =
            case Enum.at(adjacent_row, s.x + 1) do
              number ->
                list ++ [number]

              nil ->
                list
            end

          list
      end

    %{symbol: s, adjacents: list}
  end)

defmodule Parser do
  def search_left(data, current, found) do
    previous = Enum.at(data, current.y) |> Enum.at(current.x - 1)

    found =
      case previous do
        nil ->
          found

        v ->
          case Regex.match?(~r/^\d/, v.value) do
            true -> search_left(data, previous, found ++ [previous.value])
            _ -> found
          end
      end

    found
  end

  def search_right(data, current, found) do
    next = Enum.at(data, current.y) |> Enum.at(current.x + 1)

    found =
      case next do
        nil ->
          found

        v ->
          case Regex.match?(~r/^\d/, v.value) do
            true -> search_right(data, next, found ++ [next.value])
            _ -> found
          end
      end

    found
  end
end

adj_nums =
  mapped
  |> Enum.map(fn x ->
    numbers =
      x.adjacents
      |> Enum.filter(fn y -> Regex.match?(~r/^\d/, y.value) end)

    %{symbol: x.symbol, numbers: numbers}
  end)
  # |> Enum.uniq_by(fn x -> x.symbol end)
  |> Enum.flat_map(fn z ->
    stuff =
      Enum.map(z.numbers, fn v ->
        left = Parser.search_left(data, v, []) |> Enum.reverse()
        right = Parser.search_right(data, v, [])
        nums = left ++ [v.value] ++ right
        start_index = v.x - Enum.count(left)
        end_index = v.x + Enum.count(right)
        actual_number = Enum.join(nums, "") |> String.to_integer()

        %{
          # symbol: z,
          # value: v,
          # left: left,
          # right: right,
          # nums: nums,
          row_index: v.y,
          start_index: start_index,
          end_index: end_index,
          number: actual_number
        }
      end)

    stuff
  end)
  |> Enum.uniq()
  |> Enum.map(fn x -> x.number end)
  |> Enum.sum()

adj_nums
```

## Part 2

```elixir
symbols =
  for row <- data,
      cell <- row,
      cell.value != "." and !Regex.match?(~r/^\d/, cell.value) do
    cell
  end

mapped =
  symbols
  |> Enum.map(fn s ->
    list = []

    list =
      case Enum.at(data, s.y - 1) do
        nil ->
          list

        adjacent_row ->
          list =
            case Enum.at(adjacent_row, s.x - 1) do
              number ->
                list ++ [number]

              nil ->
                list
            end

          list =
            case Enum.at(adjacent_row, s.x) do
              number ->
                list ++ [number]

              nil ->
                list
            end

          list =
            case Enum.at(adjacent_row, s.x + 1) do
              number ->
                list ++ [number]

              nil ->
                list
            end

          list
      end

    list =
      case Enum.at(data, s.y) do
        nil ->
          list

        adjacent_row ->
          list =
            case Enum.at(adjacent_row, s.x - 1) do
              number ->
                list ++ [number]

              nil ->
                list
            end

          list =
            case Enum.at(adjacent_row, s.x + 1) do
              number ->
                list ++ [number]

              nil ->
                list
            end

          list
      end

    list =
      case Enum.at(data, s.y + 1) do
        nil ->
          list

        adjacent_row ->
          list =
            case Enum.at(adjacent_row, s.x - 1) do
              number ->
                list ++ [number]

              nil ->
                list
            end

          list =
            case Enum.at(adjacent_row, s.x) do
              number ->
                list ++ [number]

              nil ->
                list
            end

          list =
            case Enum.at(adjacent_row, s.x + 1) do
              number ->
                list ++ [number]

              nil ->
                list
            end

          list
      end

    %{symbol: s, adjacents: list}
  end)

adj_nums =
  mapped
  |> Enum.map(fn x ->
    numbers =
      x.adjacents
      |> Enum.filter(fn y -> Regex.match?(~r/^\d/, y.value) end)

    %{symbol: x.symbol, numbers: numbers}
  end)
  # |> Enum.uniq_by(fn x -> x.symbol end)
  |> Enum.flat_map(fn z ->
    stuff =
      Enum.map(z.numbers, fn v ->
        left = Parser.search_left(data, v, []) |> Enum.reverse()
        right = Parser.search_right(data, v, [])
        nums = left ++ [v.value] ++ right
        start_index = v.x - Enum.count(left)
        end_index = v.x + Enum.count(right)
        actual_number = Enum.join(nums, "") |> String.to_integer()

        %{
          symbol: z.symbol,
          # value: v,
          # left: left,
          # right: right,
          # nums: nums,
          row_index: v.y,
          start_index: start_index,
          end_index: end_index,
          number: actual_number
        }
      end)

    stuff
  end)
  |> Enum.filter(fn x -> x.symbol.value == "*" end)
  |> Enum.uniq()
  |> Enum.group_by(fn x -> x.symbol end)
  |> Enum.filter(fn {_key, value} -> length(value) == 2 end)
  |> Enum.map(fn {_key, value} ->
    product =
      Enum.map(value, fn x -> x.number end)
      |> Enum.reduce(1, &(&1 * &2))

    product
  end)
  |> Enum.sum()

adj_nums
```
