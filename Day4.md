# Advent of code 2023 Day 4

```elixir
Mix.install([
  {:kino, "~> 0.11.3"},
  {:math, "~> 0.7.0"}
])
```

## Data

```elixir
field = Kino.Input.file("data")
```

```elixir
value = Kino.Input.read(field)
path = Kino.Input.file_path(value.file_ref)
input_data = File.read!(path) |> String.trim()

import Math

# input_data = """
# Card 1: 41 48 83 86 17 | 83 86  6 31 17  9 48 53
# Card 2: 13 32 20 16 61 | 61 30 68 82 17 32 24 19
# Card 3:  1 21 53 59 44 | 69 82 63 72 16 21 14  1
# Card 4: 41 92 73 84 69 | 59 84 76 51 58  5 54 83
# Card 5: 87 83 26 28 32 | 88 30 70 12 93 22 82 36
# Card 6: 31 18 13 56 72 | 74 77 10 23 35 67 36 11
# """ |> String.trim()

data =
  input_data
  |> String.split("\n")
  |> Enum.with_index()
  |> Enum.map(fn {row, _row_index} ->
    [winning_numbers_cards, my_numbers] = String.split(row, " | ")

    my_numbers =
      my_numbers
      |> String.trim()
      |> String.split(" ")
      |> Enum.filter(fn x -> x != "" end)
      |> Enum.map(fn x -> String.to_integer(x) end)

    [name, winning_numbers] = String.split(winning_numbers_cards, ": ")

    winning_numbers =
      winning_numbers
      |> String.trim()
      |> String.split(" ")
      |> Enum.filter(fn x -> x != "" end)
      |> Enum.map(fn x -> String.to_integer(x) end)

    shared = my_numbers -- my_numbers -- winning_numbers

    points = Math.pow(2, Enum.count(shared) - 1) |> Kernel.trunc()

    [_, card_number] =
      String.split(name, " ")
      |> Enum.filter(fn x -> x != "" end)

    card_number =
      card_number
      |> String.trim()
      |> String.to_integer()

    %{
      name: name,
      card_number: card_number,
      my_numbers: my_numbers,
      winning_numbers: winning_numbers,
      shared: shared,
      match_count: Enum.count(shared),
      points: points,
      original: true
    }
  end)

data
```

<!-- livebook:{"output":true} -->

```
[
  %{
    name: "Card   1",
    original: true,
    my_numbers: [21, 15, 1, 43, 60, 9, 83, 81, 35, 49, 40, 38, 82, 65, 20, 4, 58, 94, 16, 89, 84,
     10, 77, 48, 76],
    winning_numbers: [81, 1, 43, 40, 49, 51, 38, 65, 36, 4],
    shared: [1, 43, 81, 49, 40, 38, 65, 4],
    points: 128,
    match_count: 8,
    card_number: 1
  },
  %{
    name: "Card   2",
    original: true,
    my_numbers: [80, 31, 89, 91, 23, 55, 36, 68, 22, 61, 66, 24, 42, 49, 33, 21, 19, 73, 29, 60, 15,
     34, 71, 10, 87],
    winning_numbers: [15, 89, 71, 17, 91, 78, 35, 55, 68, 49],
    shared: [89, 91, 55, 68, 49, 15, 71],
    points: 64,
    match_count: 7,
    card_number: 2
  },
  %{
    name: "Card   3",
    original: true,
    my_numbers: [97, 90, 75, 40, 43, 65, 92, 83, 41, 4, 47, 35, 29, 80, 68, 87, 30, 71, 98, 42, 95,
     7, 76, 69, 88],
    winning_numbers: [4, 45, 78, 42, 29, 92, 16, 90, 93, 30],
    shared: [90, 92, 4, 29, 30, 42],
    points: 32,
    match_count: 6,
    card_number: 3
  },
  %{
    name: "Card   4",
    original: true,
    my_numbers: [54, 38, 30, 40, 85, 93, 71, 10, 67, 2, 81, 57, 74, 9, 14, 5, 97, 28, 79, 95, 84,
     65, 69, 23, 21],
    winning_numbers: [81, 2, 80, 85, 14, 28, 88, 84, 74, 78],
    shared: [85, 2, 81, 74, 14, 28, 84],
    points: 64,
    match_count: 7,
    card_number: 4
  },
  %{
    name: "Card   5",
    original: true,
    my_numbers: [64, 61, 77, 92, 96, 34, 29, 44, 91, 40, 73, 20, 38, 39, 85, 27, 56, 52, 86, 41, 87,
     24, 13, 62, 30],
    winning_numbers: [40, 27, 72, 38, 99, 28, 74, 31, 45, 41],
    shared: ~c"(&\e)",
    points: 8,
    match_count: 4,
    card_number: 5
  },
  %{
    name: "Card   6",
    original: true,
    my_numbers: [67, 72, 79, 7, 54, 47, 94, 84, 98, 17, 5, 99, 82, 27, 21, 96, 23, 22, 87, 66, 36,
     26, 76, 19, 58],
    winning_numbers: [58, 67, 17, 96, 73, 9, 5, 71, 23, 87],
    shared: [67, 17, 5, 96, 23, 87, 58],
    points: 64,
    match_count: 7,
    card_number: 6
  },
  %{
    name: "Card   7",
    original: true,
    my_numbers: [7, 8, 56, 14, 58, 65, 63, 36, 54, 59, 78, 79, 11, 2, 69, 55, 61, 39, 19, 60, 4, 99,
     90, 17, 95],
    winning_numbers: [89, 70, 36, 38, 86, 50, 94, 62, 56, 3],
    shared: ~c"8$",
    points: 2,
    match_count: 2,
    card_number: 7
  },
  %{
    name: "Card   8",
    original: true,
    my_numbers: [76, 91, 70, 54, 1, 33, 11, 88, 26, 69, 64, 44, 16, 20, 46, 25, 38, 49, 15, 10, 29,
     32, 87, 8, 67],
    winning_numbers: [29, 54, 26, 8, 44, 43, 41, 15, 63, 22],
    shared: [54, 26, 44, 15, 29, 8],
    points: 32,
    match_count: 6,
    card_number: 8
  },
  %{
    name: "Card   9",
    original: true,
    my_numbers: [14, 1, 39, 15, 12, 21, 13, 25, 43, 87, 78, 67, 58, 51, 49, 97, 30, 76, 59, 65, 93,
     60, 44, 77, 90],
    winning_numbers: [41, 26, 83, 86, 96, 27, 57, 6, 92, 10],
    shared: [],
    points: 0,
    match_count: 0,
    card_number: 9
  },
  %{
    name: "Card  10",
    original: true,
    my_numbers: [43, 74, 45, 70, 18, 31, 66, 96, 21, 92, 61, 91, 55, 67, 41, 15, 77, 88, 11, 7, 8,
     93, 30, 35, 82],
    winning_numbers: [38, 40, 81, 54, 30, 61, 82, 51, 99, 71],
    shared: [61, 30, 82],
    points: 4,
    match_count: 3,
    card_number: 10
  },
  %{
    name: "Card  11",
    original: true,
    my_numbers: [70, 87, 71, 33, 25, 43, 82, 49, 30, 58, 67, 89, 95, 74, 93, 28, 99, 85, 78, 73, 10,
     75, 9, 91, 15],
    winning_numbers: [69, 47, 52, 27, 78, 17, 39, 88, 83, 71],
    shared: ~c"GN",
    points: 2,
    match_count: 2,
    card_number: 11
  },
  %{
    name: "Card  12",
    original: true,
    my_numbers: [64, 33, 13, 35, 57, 29, 81, 89, 49, 47, 37, 25, 66, 68, 20, 73, 19, 36, 39, 79, 5,
     96, 3, 95, 42],
    winning_numbers: [54, 46, 50, 79, 57, 88, 90, 61, 12, 5],
    shared: [57, 79, 5],
    points: 4,
    match_count: 3,
    card_number: 12
  },
  %{
    name: "Card  13",
    original: true,
    my_numbers: [23, 58, 95, 92, 17, 52, 84, 64, 77, 54, 20, 98, 89, 83, 4, 66, 87, 25, 27, 51, 2,
     37, 81, 56, 12],
    winning_numbers: ~c"C8>\r7&Y\n[K",
    shared: ~c"Y8",
    points: 2,
    match_count: 2,
    card_number: 13
  },
  %{
    name: "Card  14",
    original: true,
    my_numbers: [50, 47, 63, 16, 91, 41, 43, 39, 2, 95, 84, 8, 18, 23, 83, 64, 97, 48, 96, 69, 29,
     44, 1, 24, 72],
    winning_numbers: [54, 42, 51, 76, 66, 14, 74, 6, 35, 89],
    shared: [],
    points: 0,
    match_count: 0,
    card_number: 14
  },
  %{
    name: "Card  15",
    original: true,
    my_numbers: [68, 30, 85, 28, 71, 49, 2, 86, 12, 13, 7, 42, 20, 93, 66, 17, 4, 67, 19, 65, 43, 6,
     16, 75, 22],
    winning_numbers: [83, 1, 88, 31, 58, 35, 21, 62, 36, 33],
    shared: [],
    points: 0,
    match_count: 0,
    card_number: 15
  },
  %{
    name: "Card  16",
    original: true,
    my_numbers: [84, 12, 6, 44, 31, 85, 71, 50, 41, 35, 27, 38, 96, 42, 21, 9, 13, 86, 49, 91, 36,
     40, 53, 3, 93],
    winning_numbers: [3, 6, 41, 38, 71, 53, 86, 12, 49, 84],
    shared: [84, 12, 6, 71, 41, 38, 86, 49, 53, 3],
    points: 512,
    match_count: 10,
    card_number: 16
  },
  %{
    name: "Card  17",
    original: true,
    my_numbers: [19, 11, 69, 63, 25, 50, 86, 80, 26, 29, 42, 48, 52, 21, 56, 14, 6, 41, 68, 40, 1,
     61, 36, 16, 3],
    winning_numbers: [1, 14, 50, 61, 19, 68, 48, 40, 63, 69],
    shared: [19, 69, 63, 50, 48, 14, 68, 40, 1, 61],
    points: 512,
    match_count: 10,
    card_number: 17
  },
  %{
    name: "Card  18",
    original: true,
    my_numbers: [6, 5, 64, 91, 3, 10, 4, 1, 84, 76, 77, 70, 27, 59, 78, 24, 32, 92, 69, 50, 52, 54,
     82, 14, 95],
    winning_numbers: [27, 3, 77, 91, 69, 84, 14, 32, 50, 5],
    shared: [5, 91, 3, 84, 77, 27, 32, 69, 50, 14],
    points: 512,
    match_count: 10,
    card_number: 18
  },
  %{
    name: "Card  19",
    original: true,
    my_numbers: [35, 89, 49, 82, 32, 18, 71, 46, 81, 93, 95, 23, 27, 45, 96, 65, 94, 24, 70, 3, 30,
     19, 85, 56, 91],
    winning_numbers: [93, 65, 3, 23, 46, 82, 49, 95, 30, 91],
    shared: [49, 82, 46, 93, 95, 23, 65, 3, 30, 91],
    points: 512,
    match_count: 10,
    card_number: 19
  },
  %{
    name: "Card  20",
    original: true,
    my_numbers: [87, 42, 93, 34, 95, 82, 73, 83, 89, 31, 70, 98, 20, 12, 41, 61, 10, 65, 81, 71, 19,
     39, 35, 2, 5],
    winning_numbers: [73, 98, 10, 19, 2, 39, 42, 81, 93, 41],
    shared: [42, 93, 73, 98, 41, 10, 81, 19, 39, 2],
    points: 512,
    match_count: 10,
    card_number: 20
  },
  %{
    name: "Card  21",
    original: true,
    my_numbers: [68, 15, 13, 12, 20, 79, 19, 94, 99, 5, 97, 4, 16, 91, 36, 76, 47, 45, 48, 53, 3,
     46, 64, 42, 67],
    winning_numbers: [3, 97, 15, 68, 19, 48, 46, 47, 42, 91],
    shared: [68, 15, 19, 97, 91, 47, 48, 3, 46, 42],
    points: 512,
    match_count: 10,
    card_number: 21
  },
  %{
    name: "Card  22",
    original: true,
    my_numbers: [31, 73, 75, 28, 63, 87, 40, 71, 59, 5, 36, 19, 8, 60, 38, 94, 48, 37, 13, 18, 52,
     69, 57, 76, 66],
    winning_numbers: ~c"B9L$E0<GWK",
    shared: ~c"KWG$<0E9LB",
    points: 512,
    match_count: 10,
    card_number: 22
  },
  %{
    name: "Card  23",
    original: true,
    my_numbers: [36, 8, 16, 20, 85, 61, 17, 44, 38, 55, 52, 27, 35, 94, 99, 37, 74, 33, 90, 65, 30,
     28, 83, 11, ...],
    winning_numbers: [11, 15, 29, 35, 99, 68, 65, 86, 88, 50],
    shared: ~c"#cA\v",
    points: 8,
    match_count: 4,
    card_number: 23
  },
  %{
    name: "Card  24",
    original: true,
    my_numbers: [28, 1, 42, 2, 95, 38, 65, 77, 66, 12, 97, 94, 34, 55, 75, 74, 89, 18, 80, 27, 63,
     20, 87, ...],
    winning_numbers: [94, 26, 38, 83, 63, 18, 25, 20, 95, 87],
    shared: [95, 38, 94, 18, 63, 20, 87],
    points: 64,
    match_count: 7,
    card_number: 24
  },
  %{
    name: "Card  25",
    original: true,
    my_numbers: [2, 78, 80, 70, 13, 9, 48, 97, 12, 65, 29, 37, 81, 35, 79, 38, 4, 22, 59, 68, 17,
     72, ...],
    winning_numbers: [35, 79, 48, 37, 65, 59, 17, 70, 9, 78],
    shared: [78, 70, 9, 48, 65, 37, 35, 79, 59, 17],
    points: 512,
    match_count: 10,
    card_number: 25
  },
  %{
    name: "Card  26",
    original: true,
    my_numbers: [36, 85, 15, 28, 96, 61, 87, 14, 74, 67, 27, 48, 1, 26, 79, 94, 66, 35, 90, 22, 62,
     ...],
    winning_numbers: [7, 68, 37, 9, 41, 13, 82, 71, 17, 69],
    shared: [],
    points: 0,
    match_count: 0,
    card_number: 26
  },
  %{
    name: "Card  27",
    original: true,
    my_numbers: [25, 28, 89, 80, 9, 33, 18, 58, 5, 34, 68, 41, 13, 4, 65, 64, 75, 47, 55, 6, ...],
    winning_numbers: [85, 65, 73, 76, 50, 52, 58, 28, 49, 96],
    shared: [28, 58, 65],
    points: 4,
    match_count: 3,
    card_number: 27
  },
  %{
    name: "Card  28",
    original: true,
    my_numbers: [79, 6, 32, 43, 76, 86, 31, 51, 44, 47, 94, 92, 23, 34, 87, 29, 42, 69, 96, ...],
    winning_numbers: [59, 85, 80, 77, 38, 74, 92, 64, 68, 21],
    shared: ~c"\\P",
    points: 2,
    match_count: 2,
    card_number: 28
  },
  %{
    name: "Card  29",
    original: true,
    my_numbers: [52, 91, 73, 53, 97, 24, 65, 51, 59, 89, 69, 70, 21, 16, 12, 63, 30, 9, ...],
    winning_numbers: [39, 73, 96, 21, 22, 45, 72, 9, 16, 31],
    shared: [73, 21, 16, 9, 22, 45],
    points: 32,
    match_count: 6,
    card_number: 29
  },
  %{
    name: "Card  30",
    original: true,
    my_numbers: [29, 45, 92, 86, 72, 49, 14, 78, 87, 81, 18, 52, 27, 73, 48, 42, 37, ...],
    winning_numbers: [70, 41, 73, 86, 81, 18, 37, 84, 49, 93],
    shared: [86, 49, 81, 18, 73, 37],
    points: 32,
    match_count: 6,
    card_number: 30
  },
  %{
    name: "Card  31",
    original: true,
    my_numbers: [72, 32, 46, 37, 67, 92, 63, 1, 36, 96, 22, 14, 84, 35, 48, 55, ...],
    winning_numbers: [47, 38, 27, 20, 53, 19, 65, 36, 32, 76],
    shared: ~c" $",
    points: 2,
    match_count: 2,
    card_number: 31
  },
  %{
    name: "Card  32",
    original: true,
    my_numbers: [62, 81, 46, 87, 76, 47, 38, 74, 5, 12, 92, 68, 75, 52, 37, ...],
    winning_numbers: [79, 86, 28, 46, 55, 13, 58, 31, 40, 52],
    shared: [46, 52, 13, 28, 86, 55],
    points: 32,
    match_count: 6,
    card_number: 32
  },
  %{
    name: "Card  33",
    original: true,
    my_numbers: [49, 27, 32, 41, 48, 86, 47, 70, 7, 63, 65, 10, 13, 94, ...],
    winning_numbers: [73, 51, 26, 98, 78, 41, 27, 63, 70, 77],
    shared: ~c"\e)F?M3",
    points: 32,
    match_count: 6,
    card_number: 33
  },
  %{
    name: "Card  34",
    original: true,
    my_numbers: [71, 19, 6, 60, 63, 15, 32, 17, 77, 34, 10, 92, 80, ...],
    winning_numbers: [59, 12, 56, 96, 69, 89, 47, 71, 15, 14],
    shared: [71, 15, 12, 47],
    points: 8,
    match_count: 4,
    card_number: 34
  },
  %{
    name: "Card  35",
    original: true,
    my_numbers: [86, 6, 94, 1, 81, 65, 36, 13, 78, 8, 32, 49, ...],
    winning_numbers: [97, 68, 94, 87, 35, 74, 86, 71, 63, 19],
    shared: ~c"V^",
    points: 2,
    match_count: 2,
    card_number: 35
  },
  %{
    name: "Card  36",
    original: true,
    my_numbers: [8, 5, 2, 45, 10, 89, 64, 30, 95, 60, 20, ...],
    winning_numbers: [4, 60, 58, 47, 12, 77, 94, 89, 1, 82],
    shared: [89, 60, 4],
    points: 4,
    match_count: 3,
    card_number: 36
  },
  %{
    name: "Card  37",
    original: true,
    my_numbers: [73, 13, 72, 18, 37, 71, 76, 59, 91, 99, ...],
    winning_numbers: [18, 9, 92, 54, 47, 19, 81, 89, 51, ...],
    shared: [18, 92],
    points: 2,
    match_count: 2,
    card_number: 37
  },
  %{
    name: "Card  38",
    original: true,
    my_numbers: [52, 61, 91, 73, 50, 87, 92, 21, 14, ...],
    winning_numbers: [96, 40, 86, 17, 43, 19, 53, 47, ...],
    shared: ~c"+",
    points: 1,
    match_count: 1,
    card_number: 38
  },
  %{
    name: "Card  39",
    original: true,
    my_numbers: [28, 83, 13, 20, 3, 9, 77, 19, ...],
    winning_numbers: [65, 9, 73, 98, 89, 68, 57, ...],
    shared: ~c"\t",
    points: 1,
    match_count: 1,
    card_number: 39
  },
  %{
    name: "Card  40",
    original: true,
    my_numbers: [74, 5, 86, 60, 3, 62, 91, ...],
    winning_numbers: [36, 50, 72, 14, 31, 51, ...],
    shared: [],
    points: 0,
    match_count: 0,
    card_number: 40
  },
  %{
    name: "Card  41",
    original: true,
    my_numbers: [83, 68, 7, 28, 12, 3, ...],
    winning_numbers: [71, 72, 28, 29, 92, ...],
    shared: [28, 12, 72, 30, ...],
    points: 512,
    match_count: 10,
    card_number: 41
  },
  %{
    name: "Card  42",
    original: true,
    my_numbers: [20, 55, 32, 46, 67, ...],
    winning_numbers: [31, 78, 10, 67, ...],
    shared: [67, 71, 10, ...],
    points: 16,
    match_count: 5,
    card_number: 42
  },
  %{
    name: "Card  43",
    original: true,
    my_numbers: [63, 39, 99, 45, ...],
    winning_numbers: [5, 24, 93, ...],
    shared: [33, 23, ...],
    points: 8,
    match_count: 4,
    ...
  },
  %{
    name: "Card  44",
    original: true,
    my_numbers: [4, 66, 34, ...],
    winning_numbers: [21, 94, ...],
    shared: [94, ...],
    points: 512,
    ...
  },
  %{
    name: "Card  45",
    original: true,
    my_numbers: [5, 61, ...],
    winning_numbers: [37, ...],
    shared: ~c"Z:%GN\"M",
    ...
  },
  %{name: "Card  46", original: true, my_numbers: [14, ...], winning_numbers: [...], ...},
  %{name: "Card  47", original: true, my_numbers: [...], ...},
  %{name: "Card  48", original: true, ...},
  %{name: "Card  49", ...},
  %{...},
  ...
]
```

## Part 1

```elixir
data
|> Enum.map(fn x -> x.points end)
|> Enum.sum()
```

<!-- livebook:{"output":true} -->

```
17782
```

## Part 2

```elixir
defmodule Copy do
  def create_children(to_copy, copies) do
    Enum.map(to_copy, fn {a, b} -> {copies + a, b} end)
  end

  def copy([]), do: []

  def copy([{copies, match_count} | rest]) do
    {to_copy, not_to_copy} = Enum.split(rest, match_count)

    copied = create_children(to_copy, copies)

    child_copies = copy(copied ++ not_to_copy)

    [copies | child_copies]
  end
end

data
|> Enum.map(fn x -> x.match_count end)
|> Enum.map(fn match_count -> {_ = 1, match_count} end)
|> Copy.copy()
|> Enum.sum()
```

<!-- livebook:{"output":true} -->

```
8477787
```
