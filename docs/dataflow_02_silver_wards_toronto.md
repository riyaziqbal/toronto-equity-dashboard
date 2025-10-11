let
  Source = #"Bronze_25-WardNames-Numbers xlsx",
  #"Inserted Literal" = Table.AddColumn(Source, "Effective Date", each "08/14/2018", type text),
  #"Changed column type" = Table.TransformColumnTypes(#"Inserted Literal", {{"Effective Date", type date}})
in
  #"Changed column type"
