# Advent of code 2023 Day 1

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

input
|> String.split("\n")
```

<!-- livebook:{"output":true} -->

```
["zlmlk1", "vqjvxtc79mvdnktdsxcqc1sevenone", "vsskdclbtmjmvrseven6", "8jkfncbeight7seven8",
 "six8dbfrsxp", "2zpcbjdxcjfone68six", "zqmzgfivethreefdnlhpeight8798", "fivenineone6",
 "6sixzvdsprdqlftwonine", "lqztrmztwo8dg", "four6onerv2pfhm", "plvzrs5", "5282gdnc918",
 "pskjsrchjpxoneonenine96fivefour", "fivefour2hhtprpjndm4", "6qbdcfdjsd1lmldklflteight",
 "gctgdhpkkjninekj65rkqg8", "eight6gcjlsmzt5", "5chvmhmfgl7xkjfdpdbp", "5tpzpnrgpftrnine",
 "threeonehqbzktq1", "685fivetwofour4lvgxgdb", "9jdxljkfqttstqxdzdsztsxrfjbkqmmsqzseven",
 "twostcllbpndtwo15seven", "8fivetwofivetjfvsxzs5kdkpxgxvsfhr7", "sevenlvrc2fivefivesixqkzdkrfour",
 "45jjpmnscmmk", "lhxgbfjtcknpvz6", "tndgnkcqtjbrzgbrfjr3fiveqxlktntzthree", "vqb6threeeightbdt",
 "1eighteight7fourone8", "bcmqn9onecnrzhsrsgzggzhtskjeightbz6khfhccktwonenrj",
 "6qmgkbkmlxfourprhxrxrdseight", "9three479", "9two6zhtjzfmjrteight",
 "1fiverrxdmvfvxhs7jqzzqpcflzt75", "pnjmlpbbeightskgdf6one", "6cpzqzfsjtpfq135", "484",
 "llfvhxglfivesixthreenseven36", "jhcpt9rq7fhzbnhk", "tthree5lrgtbxxvonezfmdpseven2",
 "67jmrxfdmfbmzsixkzghng", "sixthreefdbzhslqone2sevenfoursevennlnpjgsx", "54zrkfbfq",
 "six7rfpfbzbghxcnxlnfjkznine7", "8dfoursm338cz", "sevenfiveeightone68", "dqmvcbdclx23653",
 "eight58qgjlcvflrggndskff", ...]
```

## Part 1

```elixir
numbers =
  input
  |> String.split("\n")
  |> Enum.map(fn x ->
    numbers =
      String.graphemes(x)
      |> Enum.filter(fn y -> Regex.match?(~r/^\d+$/, y) end)
      |> Enum.map(fn y -> String.to_integer(y) end)

    numbers
  end)

Enum.map(numbers, fn y ->
  if !is_list(y) do
    y
  else
    first = List.first(y)
    last = Enum.reverse(y) |> List.first()
    String.to_integer("#{first}#{last}")
  end
end)
|> Enum.sum()
```

<!-- livebook:{"output":true} -->

```
54697
```

## Part 2

```elixir
defmodule Helpers do
  def map_to_number(text, list) do
    cond do
      Regex.match?(~r/^\d/, text) ->
        number = Regex.run(~r/^\d/, text) |> Enum.at(0)
        list = list ++ [String.to_integer(number)]
        text = String.slice(text, 1..-1)
        map_to_number(text, list)

      String.starts_with?(text, "one") ->
        list = list ++ [1]
        text = String.slice(text, 1..-1)
        map_to_number(text, list)

      String.starts_with?(text, "two") ->
        list = list ++ [2]
        text = String.slice(text, 1..-1)
        map_to_number(text, list)

      String.starts_with?(text, "three") ->
        list = list ++ [3]
        text = String.slice(text, 1..-1)
        map_to_number(text, list)

      String.starts_with?(text, "four") ->
        list = list ++ [4]
        text = String.slice(text, 1..-1)
        map_to_number(text, list)

      String.starts_with?(text, "five") ->
        list = list ++ [5]
        text = String.slice(text, 1..-1)
        map_to_number(text, list)

      String.starts_with?(text, "six") ->
        list = list ++ [6]
        text = String.slice(text, 1..-1)
        map_to_number(text, list)

      String.starts_with?(text, "seven") ->
        list = list ++ [7]
        text = String.slice(text, 1..-1)
        map_to_number(text, list)

      String.starts_with?(text, "eight") ->
        list = list ++ [8]
        text = String.slice(text, 1..-1)
        map_to_number(text, list)

      String.starts_with?(text, "nine") ->
        list = list ++ [9]
        text = String.slice(text, 1..-1)
        map_to_number(text, list)

      String.length(text) > 0 ->
        text = String.slice(text, 1..-1)
        map_to_number(text, list)

      true ->
        list
    end
  end
end

numbers =
  input
  |> String.split("\n")
  |> Enum.map(fn x ->
    Helpers.map_to_number(x, [])
  end)

Enum.map(numbers, fn y ->
  if !is_list(y) do
    y
  else
    first = List.first(y)
    last = Enum.reverse(y) |> List.first()
    String.to_integer("#{first}#{last}")
  end
end)
# |> IO.inspect()
|> Enum.sum()
```

<!-- livebook:{"output":true} -->

```
54885
```
