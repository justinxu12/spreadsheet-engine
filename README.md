# Spreadsheet Engine

A spreadsheet computation engine built in Python with support for formula evaluation, dependency tracking, multiple worksheets, cell ranges, spreadsheet functions, and automatic recalculation.

The engine parses spreadsheet formulas using a custom Lark grammar, tracks relationships between cells through a dependency graph, and automatically recalculates affected cells when spreadsheet data changes.

## Features

### Formula Evaluation

Supports spreadsheet-style expressions including:

- Arithmetic operations: `+`, `-`, `*`, `/`
- Comparison operations: `=`, `==`, `<>`, `!=`, `>`, `<`, `>=`, `<=`
- String concatenation using `&`
- Boolean values
- Cell and cell-range references
- Cross-sheet references
- Absolute and mixed references such as `$A$1`, `$A1`, and `A$1`
- Spreadsheet error values and error propagation

### Dependency Tracking

Cell relationships are maintained through a dependency graph. When a cell changes, dependent cells are identified and recalculated automatically.

The engine also detects circular dependencies and represents them using spreadsheet-style circular-reference errors.

### Spreadsheet Functions

Includes support for common spreadsheet functions such as:

- `SUM`, `AVERAGE`, `MIN`, `MAX`
- `IF`, `IFERROR`, `CHOOSE`
- `AND`, `OR`, `NOT`, `XOR`
- `EXACT`, `ISBLANK`, `ISERROR`
- `INDIRECT`
- `HLOOKUP`, `VLOOKUP`

### Workbook Management

Workbooks support:

- Creating and deleting worksheets
- Renaming, reordering, and copying worksheets
- Saving and loading workbooks
- Retrieving worksheet dimensions
- Registering notifications for changed cells

### Cell and Region Operations

The engine supports higher-level spreadsheet operations including:

- Copying cell regions
- Moving cell regions
- Sorting spreadsheet regions
- Cell ranges
- Relative, absolute, and mixed cell references

## Example

```python
from sheets import Workbook

# Create a workbook and worksheet
workbook = Workbook()
_, sheet_name = workbook.new_sheet("Sheet1")

# Add values
workbook.set_cell_contents(sheet_name, "A1", "10")
workbook.set_cell_contents(sheet_name, "A2", "20")

# Add a formula
workbook.set_cell_contents(sheet_name, "A3", "=A1+A2")

print(workbook.get_cell_value(sheet_name, "A3"))
# 30

# Update a referenced cell
workbook.set_cell_contents(sheet_name, "A1", "25")

print(workbook.get_cell_value(sheet_name, "A3"))
# 45
```

Dependent formulas are automatically recalculated when referenced cells change.

## Architecture

The engine separates workbook management, formula parsing, expression evaluation, and dependency tracking into dedicated components:

```text
sheets/
├── Workbook.py              # Workbook management and public spreadsheet API
├── Sheet.py                 # Worksheet representation and cell coordinates
├── Cell.py                  # Individual spreadsheet cells
├── CellValue.py             # Cell value representation
├── CellError.py             # Spreadsheet error types
├── DependencyGraph.py       # Cell dependency tracking
├── formulas.lark            # Spreadsheet formula grammar
├── interpreter.py           # Formula evaluation
├── transformer.py           # Parse-tree transformations
├── visitor.py               # Formula tree traversal
├── SpreadsheetFunctions.py  # Built-in spreadsheet functions
└── RowAdapter.py            # Row operations used by region sorting
```

Formula strings are parsed into syntax trees using Lark. Cell references discovered during evaluation are tracked in the dependency graph, allowing the workbook to determine which cells need to be recalculated after an update.

## Testing

The project includes unit and performance tests covering formula evaluation, spreadsheet operations, errors, functions, workbook serialization, dependency updates, and cycle handling.

```text
tests/
├── unit/
│   ├── test_basic.py
│   ├── test_errors.py
│   ├── test_functions.py
│   ├── test_json.py
│   ├── test_smoke.py
│   └── test_spreadsheet.py
└── performance/
    ├── test_cycles.py
    ├── test_performance.py
    ├── test_updates.py
    └── test_workbook.py
```

## Tech Stack

- **Python** — core spreadsheet engine
- **Lark** — formula parsing
- **Coverage.py** — test coverage
- **GitHub Actions** — continuous integration

## Author

Justin Xu
