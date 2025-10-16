let
  Source = Lakehouse.Contents([]),
  Navigation = Source{[workspaceId = "6a2d5314-0cf7-4016-a2f6-97ffae7192ad"]}[Data],
  #"Navigation 1" = Navigation{[lakehouseId = "30fdebf4-ca3e-422a-8387-5f4d13fe2abf"]}[Data],
  #"Navigation 2" = #"Navigation 1"{[Id = "Files", ItemKind = "Folder"]}[Data],
  #"Navigation 3" = #"Navigation 2"{[Name = "TTC Data"]}[Content],
  #"Navigation 4" = #"Navigation 3"{[Name = "stops.txt"]}[Content],
  #"Imported CSV" = Csv.Document(#"Navigation 4"),
  #"Promoted headers" = Table.PromoteHeaders(#"Imported CSV", [PromoteAllScalars = true]),
  #"Changed column type" = Table.TransformColumnTypes(#"Promoted headers", {{"stop_id", Int64.Type}, {"stop_code", type text}, {"stop_name", type text}, {"stop_desc", type text}, {"stop_lat", type number}, {"stop_lon", type number}, {"zone_id", type text}, {"stop_url", type text}, {"location_type", type text}, {"parent_station", type text}, {"stop_timezone", type text}, {"wheelchair_boarding", Int64.Type}}),
  #"Choose columns" = Table.SelectColumns(#"Changed column type", {"stop_id", "stop_code", "stop_name", "stop_desc", "stop_lat", "stop_lon", "zone_id", "stop_url", "location_type", "parent_station", "stop_timezone", "wheelchair_boarding"}),
  #"Changed column type 1" = Table.TransformColumnTypes(#"Choose columns", {{"stop_code", type text}})
in
  #"Changed column type 1"