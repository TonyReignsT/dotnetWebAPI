### NOTES

### Evidence from the end of the module Create a web API with ASP.NET Core controllers showing the existing pizzas and your additional pizza:

### GET/pizza:

HTTP/1.1 200 OK
Connection: close
Content-Type: application/json; charset=utf-8
Server: Kestrel
Transfer-Encoding: chunked

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
    "name": "Chicken BBQ",
    "isGlutenFree": false
  }
]

### POST Operation
**Request:** POST /pizza
**Request Body:**
{
    "name": "Hawaii",
    "isGlutenFree": false
}

**Response:**
HTTP/1.1 201 Created
Connection: close
Content-Type: application/json; charset=utf-8
Server: Kestrel
Location: http://localhost:5108/Pizza/4
Transfer-Encoding: chunked

{
  "id": 4,
  "name": "Hawaii",
  "isGlutenFree": false
}

### PUT Operation
**Request:** PUT /pizza/4
**Request Body:**
{
    "id": 4,
    "name": "Hawaiian",
    "isGlutenFree": false
}
**Response:**
HTTP/1.1 204 No Content
Connection: close
Server: Kestrel

#### DELETE Operation
**Request:** DELETE /pizza/4
**Response:**
HTTP/1.1 204 No Content
Connection: close



### working sales summary function from the Work with files and directories in a .NET app module.
using System.Text;

void GenerateSalesSummaryReport(IEnumerable<string> files, string outputFilePath)
{
    double grandTotal = 0;
    StringBuilder reportBuilder = new StringBuilder();
    StringBuilder detailsBuilder = new StringBuilder();

    // Looping through files to get individual totals for the "Details" section
    foreach (var file in files)
    {
        string salesJson = File.ReadAllText(file);
        SalesData? data = JsonConvert.DeserializeObject<SalesData?>(salesJson);
        double fileTotal = data?.Total ?? 0;
        grandTotal += fileTotal;

        // Extracting just the file name (e.g., "sales.json") for a cleaner report
        string fileName = Path.GetFileName(file);
        detailsBuilder.AppendLine($"  {fileName}: {fileTotal:C}");
    }

    // Final report format
    reportBuilder.AppendLine("Sales Summary");
    reportBuilder.AppendLine("----------------------------");
    reportBuilder.AppendLine($"  Total Sales: {grandTotal:C}");
    reportBuilder.AppendLine();
    reportBuilder.AppendLine("  Details:");
    reportBuilder.Append(detailsBuilder.ToString());

    // Writing the formatted string to the file
    File.WriteAllText(outputFilePath, reportBuilder.ToString());
}