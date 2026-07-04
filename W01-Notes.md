# W01 Notes

## Web API Evidence

### GET /pizza

Status code: 200 OK

Response included the existing pizzas and my additional record:

```json
[
  {
    "id": 1,
    "name": "Classic Italian",
    "isGlutenFree": false
  },
  {
    "id": 2,
    "name": "Veggie",
    "isGlutenFree": true
  },
  {
    "id": 3,
    "name": "Pepperoni",
    "isGlutenFree": false
  }
]
````

### POST /pizza

Status code: 201 Created

```json
{
  "id": 4,
  "name": "Hawaii",
  "isGlutenFree": false
}
```

### PUT /pizza/4

Status code: 204 No Content

### GET /pizza/4

Status code: 200 OK

```json
{
  "id": 4,
  "name": "Hawaiian",
  "isGlutenFree": false
}
```

### DELETE /pizza/4

Status code: 204 No Content

### GET /pizza after DELETE

Status code: 200 OK

```json
[
  {
    "id": 1,
    "name": "Classic Italian",
    "isGlutenFree": false
  },
  {
    "id": 2,
    "name": "Veggie",
    "isGlutenFree": true
  },
  {
    "id": 3,
    "name": "Pepperoni",
    "isGlutenFree": false
  }
]
```

## Sales Summary Function

```csharp
void GenerateSalesSummaryReport(IEnumerable<string> salesFiles, string outputFile)
{
    double grandTotal = 0;
    StringBuilder report = new StringBuilder();

    report.AppendLine("Sales Summary");
    report.AppendLine("----------------------------");

    List<(string FileName, double Total)> details = new();

    foreach (var file in salesFiles)
    {
        string salesJson = File.ReadAllText(file);

        SalesData? data = JsonConvert.DeserializeObject<SalesData>(salesJson);

        double total = data?.Total ?? 0;

        grandTotal += total;

        string store = Path.GetFileName(Path.GetDirectoryName(file)!);
        details.Add(($"{store}/sales.json", total));
    }

    report.AppendLine($" Total Sales: {grandTotal:C}");
    report.AppendLine();
    report.AppendLine(" Details:");

    foreach (var item in details)
    {
        report.AppendLine($"  {item.FileName}: {item.Total:C}");
    }

    File.WriteAllText(outputFile, report.ToString());
}
```