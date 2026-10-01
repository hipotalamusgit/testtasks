# Power BI: True/False column to exclude money transfers by transaction type

Assumes a table `Transactions` with a text column `TransactionType`.
Rename the table, column and keyword list to match your model.

## Option 1 - DAX calculated column

Modeling -> New column:

```dax
Exclude Money Transfer =
VAR _type = LOWER ( TRIM ( Transactions[TransactionType] ) )
RETURN
    _type IN { "money transfer", "transfer", "p2p transfer", "internal transfer" }
        || CONTAINSSTRING ( _type, "transfer" )
```

Returns `TRUE` for money-transfer types (to exclude) and `FALSE` for everything else.
Blank types return `FALSE`. If you only want exact matches, delete the `CONTAINSSTRING` line.

## Option 2 - Power Query (M) custom column

Transform data -> Add Column -> Custom Column, or paste as a step:

```m
= Table.AddColumn(
    #"Previous Step",
    "Exclude Money Transfer",
    each
        let t = Text.Lower(Text.Trim(Text.From([TransactionType]) ?? ""))
        in List.Contains({"money transfer", "transfer", "p2p transfer", "internal transfer"}, t)
            or Text.Contains(t, "transfer"),
    type logical
)
```

Replace `#"Previous Step"` with the name of the preceding step in your query.

## Using the column

- **Report/page/visual filter:** drag `Exclude Money Transfer` to the Filters pane and keep `False`.
- **Slicer:** add it as a slicer so users can toggle money transfers in or out.
- **Measure that always ignores transfers:**

```dax
Total Amount (excl. Transfers) =
CALCULATE (
    SUM ( Transactions[Amount] ),
    Transactions[Exclude Money Transfer] = FALSE ()
)
```
