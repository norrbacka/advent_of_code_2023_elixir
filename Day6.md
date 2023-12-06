# Advent of code 2023 Day 6

```elixir
Mix.install([
  {:kino, "~> 0.11.3"},
  {:math, "~> 0.7.0"}
])
```

## Data

```elixir
input_data =
  """
  Time:        54     81     70     88
  Distance:   446   1292   1035   1007
  """
  |> String.trim()

test_data =
  """
  Time:      7  15   30
  Distance:  9  40  200
  """
  |> String.trim()

[time, distance] =
  input_data
  |> String.split("\n")
  |> Enum.filter(fn x -> x != "" end)
  |> Enum.with_index()
  |> Enum.map(fn {row, _row_index} ->
    row
    |> String.split(":")
    |> Enum.map(fn x ->
      String.trim(x)
      |> String.split("\s")
      |> Enum.filter(fn y -> y != "" end)
    end)
    |> List.last()
    |> Enum.map(fn y -> String.to_integer(y) end)
  end)

time_and_distance =
  Enum.to_list(0..(length(time) - 1))
  |> Enum.map(fn x -> %{time: Enum.at(time, x), distance: Enum.at(distance, x)} end)
```

<!-- livebook:{"output":true} -->

```
warning: outdented heredoc line. The contents inside the heredoc should be indented at the same level as the closing """. The following is forbidden:

    def text do
      """
    contents
      """
    end

Instead make sure the contents are indented as much as the heredoc closing:

    def text do
      """
      contents
      """
    end

The current heredoc line is indented too little
  #cell:m2lwykwl6ddpjmjvkjd77p6wyyqetgac:3:3

warning: outdented heredoc line. The contents inside the heredoc should be indented at the same level as the closing """. The following is forbidden:

    def text do
      """
    contents
      """
    end

Instead make sure the contents are indented as much as the heredoc closing:

    def text do
      """
      contents
      """
    end

The current heredoc line is indented too little
  #cell:m2lwykwl6ddpjmjvkjd77p6wyyqetgac:10:3

```

<!-- livebook:{"output":true} -->

```
[
  %{time: 54, distance: 446},
  %{time: 81, distance: 1292},
  %{time: 70, distance: 1035},
  %{time: 88, distance: 1007}
]
```

## Part 1

```elixir
import Math

time_and_distance
|> Enum.map(fn x ->
  Enum.map(0..x.time, fn speed ->
    distance_left = x.time - speed
    traveled = distance_left * speed

    %{
      distance: x.distance,
      time: x.time,
      distance_left: distance_left,
      traveled: traveled
    }
  end)
  |> Enum.filter(fn x -> x.traveled > x.distance end)
  |> Enum.count()
end)
|> Enum.reduce(1, fn x, acc -> x * acc end)
```

<!-- livebook:{"output":true} -->

```
2065338
```

## Part 2

```elixir
time =
  time_and_distance
  |> Enum.map(fn x -> x.time end)
  |> Enum.join("")
  |> String.to_integer()

distance =
  time_and_distance
  |> Enum.map(fn x -> x.distance end)
  |> Enum.join("")
  |> String.to_integer()

Stream.map(0..time, fn hold ->
  remaining = time - hold
  hold * remaining
end)
|> Enum.filter(&(distance < &1))
|> Enum.count()
```

<!-- livebook:{"output":true} -->

```
warning: variable time_and_distance in code block has no effect as it is never returned (remove the variable or assign it to _ to avoid warnings)
  #cell:3lwecamrzckojw5b7yttrwjdnbvlpzcf:1

```

<!-- livebook:{"output":true} -->

```
34934171
```
