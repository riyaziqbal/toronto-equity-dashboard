let
  Source = Lakehouse.Contents(null),
  Navigation = Source{[workspaceId = "6a2d5314-0cf7-4016-a2f6-97ffae7192ad"]}[Data],
  #"Navigation 1" = Navigation{[lakehouseId = "30fdebf4-ca3e-422a-8387-5f4d13fe2abf"]}[Data],
  #"Navigation 2" = #"Navigation 1"{[Id = "Files", ItemKind = "Folder"]}[Data],
  #"Navigation 3" = #"Navigation 2"{[Name = "Census_Data"]}[Content],
  #"Navigation 4" = #"Navigation 3"{[Name = "25-WardNames-Numbers.xlsx"]}[Content],
  #"Imported Excel workbook" = Excel.Workbook(#"Navigation 4", null, true),
  #"Navigation 5" = #"Imported Excel workbook"{[Item = "25 Ward Names and Numbers", Kind = "Sheet"]}[Data],
  #"Promoted headers" = Table.PromoteHeaders(#"Navigation 5", [PromoteAllScalars = true]),
  #"Changed column type" = Table.TransformColumnTypes(#"Promoted headers", {{"Ward Number", Int64.Type}, {"Ward Name", type text}})
in
  #"Changed column type"
