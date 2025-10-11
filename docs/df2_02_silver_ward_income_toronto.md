let
  Source = #"Bronze 2023-WardProfiles-2011-2021-CensusData xlsx",
  #"Removed columns" = Table.RemoveColumns(Source, {"RowOrder"}),
  #"Removed blank rows" = Table.SelectRows(#"Removed columns", each not List.IsEmpty(List.RemoveMatchingItems(Record.FieldValues(_), {"", null}))),
  #"Promoted headers" = Table.PromoteHeaders(#"Removed blank rows", [PromoteAllScalars = true]),
  startIndex = List.PositionOf(List.Transform(Table.Column(#"Promoted headers", "Column1"), each Text.Contains(Text.From(_), "Average total income")), true),
  trimmedTable = Table.FirstN(Table.Skip(#"Promoted headers", startIndex), 2),
  #"Unpivoted other columns" = Table.UnpivotOtherColumns(trimmedTable, {"Column1"}, "Attribute", "Value"),
  #"Filtered rows" = Table.SelectRows(#"Unpivoted other columns", each ([Attribute] <> "Toronto")),
  #"Replaced value" = Table.ReplaceValue(#"Filtered rows", "Average total income of households in 2020 ($)", "Average Total Income", Replacer.ReplaceText, {"Column1"}),
  #"Replaced value 1" = Table.ReplaceValue(#"Replaced value", "Median total income of households in 2020 ($)", "Median Total Income", Replacer.ReplaceText, {"Column1"}),
  #"Renamed columns" = Table.RenameColumns(#"Replaced value 1", {{"Column1", "Income Type"}, {"Attribute", "Ward"}, {"Value", "CAD"}}),
  #"Choose columns" = Table.SelectColumns(#"Renamed columns", {"Income Type", "Ward", "CAD"}),
  #"Typed Columns" = Table.TransformColumnTypes(#"Choose columns", {{"Income Type", type text}, {"Ward", type text}, {"CAD", Currency.Type}})
in
  #"Typed Columns"
