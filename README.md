# Olo---Pizza-Test
populate 20 most frequently ordered pizza toppings

type Pizza = JsonProvider<"pizzas.json">
type ToppingFrequency = { Toppings: string[]; Frequency: int }

let getToppingFrequency grouping =
  {Toppings = fst grouping; Frequency = snd grouping |> Array.length}
  
  [<EntryPoint>]
  let main argv =
    let bestToppings =
      Pizza.Load " http://files.olo.com/pizzas.json"
      |> Array.groupBy (fun x -> x.Toppings |> Array.sort)
      |> Array.map getToppingFrequency
      |> Array.sortByDescending (fun x -> x.Frequency)
      |> Array.take 20
  
  bestToppings |> Array.map (fun x -> printfn "%A" x) |> ignore
  0 // return an integer exit code
