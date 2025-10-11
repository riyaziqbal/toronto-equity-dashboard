let
  // Connect to Lakehouse and navigate to the Excel file
  Source = Lakehouse.Contents(null),
  Navigation = Source{[workspaceId = "6a2d5314-0cf7-4016-a2f6-97ffae7192ad"]}[Data],
  Lakehouse = Navigation{[lakehouseId = "30fdebf4-ca3e-422a-8387-5f4d13fe2abf"]}[Data],
  FilesFolder = Lakehouse{[Id = "Files", ItemKind = "Folder"]}[Data],
  CensusDataFolder = FilesFolder{[Name = "Census_Data"]}[Content],
  ExcelFile = CensusDataFolder{[Name = "2023-WardProfiles-2011-2021-CensusData.xlsx"]}[Content],

  // Load the sheet and skip header rows
  ImportedWorkbook = Excel.Workbook(ExcelFile, null, true),
  RawSheet = ImportedWorkbook{[Item = "2021 One Variable", Kind = "Sheet"]}[Data],
  SkippedTopRows = Table.Skip(RawSheet, 16),

  // Promote headers
  PromotedHeaders = Table.PromoteHeaders(SkippedTopRows, [PromoteAllScalars=true]),

  // Add index for row order
  AddedIndex = Table.AddIndexColumn(PromotedHeaders, "RowOrder", 1, 1, Int64.Type),

  // Convert all columns except RowOrder to text
  TextColumns = List.RemoveItems(Table.ColumnNames(AddedIndex), {"RowOrder"}),
  AllTextColumns = Table.TransformColumnTypes(AddedIndex, List.Transform(TextColumns, each {_, type text})),

  // Remove unwanted columns
  CleanedColumns = Table.RemoveColumns(AllTextColumns, {
    "Column28", "Column29", "Column30", "Column31", "Column32", "Column33", "Column34",
    "Column35", "Column36", "Column37", "Column38", "Column39", "Column40", "Column41",
    "Column42", "Column43", "Column44", "Column45", "Column46", "Column47", "Column48",
    "Column49", "Column50"
  }),

  // Remove blank rows
  NonBlankRows = Table.SelectRows(CleanedColumns, each not List.IsEmpty(List.RemoveMatchingItems(Record.FieldValues(_), {"", null}))),

  // Sort by numeric RowOrder
  SortedFinal = Table.Sort(NonBlankRows, {{"RowOrder", Order.Ascending}}),
  #"Choose columns" = Table.SelectColumns(SortedFinal, {"Population", "Column2", "Column3", "Column4", "Column5", "Column6", "Column7", "Column8", "Column9", "Column10", "Column11", "Column12", "Column13", "Column14", "Column15", "Column16", "Column17", "Column18", "Column19", "Column20", "Column21", "Column22", "Column23", "Column24", "Column25", "Column26", "Column27", "RowOrder"})
in
  #"Choose columns"
