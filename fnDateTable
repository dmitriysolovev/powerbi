let fnDateTable = (StartDate as date, EndDate as date, optional Culture as nullable text) as table =>
  let
    DayCount = Duration.Days(Duration.From(EndDate - StartDate))+1,
    Source = List.Dates(StartDate,DayCount,#duration(1,0,0,0)),
    TableFromList = Table.FromList(Source, Splitter.SplitByNothing()),    
    ChangedType = Table.TransformColumnTypes(TableFromList,{{"Column1", type date}}),
    RenamedColumns = Table.RenameColumns(ChangedType,{{"Column1", "Date"}}),
    InsertYear = Table.AddColumn(RenamedColumns, "Year", each Date.Year([Date]),type text),
    InsertQuarterNum = Table.AddColumn(InsertYear, "Quarter Num", each Date.QuarterOfYear([Date])),
    InsertQuarter = Table.AddColumn(InsertQuarterNum, "Quarter", each "Q" & Number.ToText([Quarter Num])),
    InsertMonth = Table.AddColumn(InsertQuarter, "Month Num", each Date.Month([Date]), type text),
    InsertStartOfMonth = Table.AddColumn(InsertMonth, "StartOfMonth", each Date.StartOfMonth([Date]), type date),
    InsertEndOfMonth = Table.AddColumn(InsertStartOfMonth, "EndOfMonth", each Date.EndOfMonth([Date]), type date),
    InsertDay = Table.AddColumn(InsertEndOfMonth, "DayOfMonth", each Date.Day([Date])),
    InsertDayInt = Table.AddColumn(InsertDay, "DateInt", each [Year] * 10000 + [Month Num] * 100 + [DayOfMonth]),
    InsertMonthName = Table.AddColumn(InsertDayInt, "Month", each Date.ToText([Date], "MMMM", Culture), type text),
    InsertShortMonthName = Table.AddColumn(InsertMonthName, "Month short", each Date.ToText([Date], "MMM", Culture), type text),
    InsertCalendarMonth = Table.AddColumn(InsertShortMonthName, "Month Year", each [Month short]& " " & Number.ToText([Year]),type text),
    InsertCalendarQtr = Table.AddColumn(InsertCalendarMonth, "Quarter Year", each "Q" & Number.ToText([Quarter Num]) & " " & Number.ToText([Year]), type text),
    InsertDayWeek = Table.AddColumn(InsertCalendarQtr, "Weekday Num", each Date.DayOfWeek([Date])),
    InsertDayName = Table.AddColumn(InsertDayWeek, "Weekday", each Date.ToText([Date], "dddd", Culture), type text),
    InsertShortDayName = Table.AddColumn(InsertDayName, "Weekday short", each Date.ToText([Date], "ddd", Culture), type text),
    InsertWeekEnding = Table.AddColumn(InsertShortDayName , "EndOfWeek", each Date.EndOfWeek([Date]), type date),
    InsertWeekNumber= Table.AddColumn(InsertWeekEnding, "Week Num", each Date.WeekOfYear([Date])),
    InsertMonthWeekNumber= Table.AddColumn(InsertWeekNumber, "WeekOfMonth Num", each Date.WeekOfMonth([Date])),
    InsertMonthnYear = Table.AddColumn(InsertMonthWeekNumber,"Month-YearOrder", each [Year] * 10000 + [Month Num] * 100),
    InsertQuarternYear = Table.AddColumn(InsertMonthnYear,"Quarter-YearOrder", each [Year] * 10000 + [Quarter Num] * 100),
    ChangedType1 = Table.TransformColumnTypes(InsertQuarternYear,{{"Quarter-YearOrder", Int64.Type},{"Week Num", Int64.Type},{"WeekOfMonth Num", Int64.Type},{"Quarter", type text},{"Year", type text},{"Month-YearOrder", Int64.Type}, {"DateInt", Int64.Type}, {"DayOfMonth", Int64.Type}, {"Month Num", Int64.Type}, {"Quarter Num", Int64.Type}, {"Weekday Num", Int64.Type}})
  in
    ChangedType1
in
    fnDateTable
