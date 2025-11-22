let
  Source = Lakehouse.Contents([]),
  #"Navigation 1" = Source{[workspaceId = "6a2d5314-0cf7-4016-a2f6-97ffae7192ad"]}[Data],
  #"Navigation 2" = #"Navigation 1"{[lakehouseId = "30fdebf4-ca3e-422a-8387-5f4d13fe2abf"]}[Data],
  #"Navigation 3" = #"Navigation 2"{[Id = "Files", ItemKind = "Folder"]}[Data],
  #"Navigation 4" = #"Navigation 3"{[Name = "TTC Data"]}[Content],
  #"Navigation 5" = #"Navigation 4"{[Name = "shapes.txt"]}[Content],
  #"Imported CSV" = Csv.Document(#"Navigation 5"),
  #"Promoted headers" = Table.PromoteHeaders(#"Imported CSV", [PromoteAllScalars = true]),
  #"Changed column type" = Table.TransformColumnTypes(#"Promoted headers", {{"shape_id", Int64.Type}, {"shape_pt_lat", type number}, {"shape_pt_lon", type number}, {"shape_pt_sequence", Int64.Type}, {"shape_dist_traveled", type number}})
in
  #"Changed column type"
