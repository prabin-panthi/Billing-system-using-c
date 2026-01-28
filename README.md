# Restaurant Billing System

A command-line billing system for restaurants built in C. Generate invoices, save them to file, view all invoices, and search for specific customer bills.

## What This Does

A complete billing system that:
- Creates itemized invoices with customer name, date, and time
- Calculates subtotal, discount, VAT, and grand total
- Saves invoices to a text file
- Displays all saved invoices
- Searches for specific customer invoices

## Features

1. **Generate Invoice** - Create new bill with items, quantities, and prices
2. **Show All Invoices** - View all saved bills from file
3. **Search Invoice** - Find bills by customer name
4. **Auto-calculations** - Discount (10%), VAT (13%), and totals
5. **File Storage** - Saves bills to `Billing.txt`

## How to Compile and Run

### Compile:
```bash
gcc billing.c -o billing
```

### Run:
```bash
./billing
```

Or on Windows:
```bash
billing.exe
```

## Menu Options

```
1. Generate Invoice
2. Show all Invoices
3. Search for Invoice
4. Exit
```

## Usage

### Option 1: Generate Invoice

1. Select option `1`
2. Enter customer name
3. Enter number of items
4. For each item, enter:
   - Item name
   - Quantity
   - Price per unit
5. Bill displays with calculations
6. Choose to save (y/n)

**Example:**
```
Enter name of customer: John Doe
Enter number of items: 2
For item 1
Enter item name: Pizza
Enter quantity: 2
Enter price of unit item: 500
For item 2
Enter item name: Coke
Enter quantity: 3
Enter price of unit item: 50
```

### Option 2: Show All Invoices

- Displays all saved invoices from `Billing.txt`
- Shows complete bill details for each customer

### Option 3: Search Invoice

- Enter customer name
- System searches `Billing.txt`
- Displays matching invoice if found

### Option 4: Exit

- Exits the program with thank you message

## Bill Format

```
                   ***WRC RESTRO***
----------------------------------------------------------------------

Date: Jan 28 2026              Time: 14:30:45
Invoice To: John Doe

----------------------------------------------------------------------
Items                    Qty              Total
----------------------------------------------------------------------

Pizza                    2                1000.00
Coke                     3                150.00

----------------------------------------------------------------------
Sub total                                 1150.00
Discount @10%                             115.00
                                          ------
Net total                                 1035.00
Vat @13%                                  134.55

----------------------------------------------------------------------
Grand Total                               1169.55
----------------------------------------------------------------------~
```

## Code Structure

### Main Function
- Displays menu
- Handles user choice with switch-case
- Controls program flow

### Functions

**billHead()**
```c
void billHead(char name[], char date[], char time[])
```
- Prints invoice header
- Shows restaurant name, date, time, customer name
- Displays column headers

**billBody()**
```c
int billBody(char items[], int qty, int price)
```
- Prints each item line
- Returns total for that item
- Format: Item name, quantity, total price

**billFoot()**
```c
void billFoot(int sum)
```
- Prints invoice footer
- Calculates and displays:
  - Subtotal
  - Discount (10%)
  - Net total
  - VAT (13%)
  - Grand total

### Data Structure

```c
struct body {
    char items[100];  // Item name
    int qty;          // Quantity
    int price;        // Unit price
};
```

## Calculations

**Subtotal:** Sum of all item totals
```c
subtotal = Σ(quantity × price)
```

**Discount:** 10% of subtotal
```c
discount = subtotal × 0.10
```

**Net Total:** Subtotal minus discount
```c
net_total = subtotal - discount
```

**VAT:** 13% of net total
```c
vat = net_total × 0.13
```

**Grand Total:** Net total plus VAT
```c
grand_total = net_total + vat
```

## File Operations

### Save Invoice (Option 1)
```c
freopen("Billing.txt", "a", stdout);
// Prints bill to file instead of console
```
Uses append mode to add new invoices without overwriting.

### Read All Invoices (Option 2)
```c
fptr = fopen("Billing.txt","r");
while(fgets(str,100000,fptr) != NULL) {
    printf("%s", str);
}
```

### Search Invoice (Option 3)
- Reads entire file into string
- Searches for customer name
- Finds bill boundaries (starts at name, ends at '~')
- Displays matching invoice

## Special Features

### Auto-capitalization
First letter of item name is automatically capitalized:
```c
if(goods[i].items[0] >= 'a' && goods[i].items[0] <= 'z') {
    goods[i].items[0] = goods[i].items[0] - 32;
}
```

### Date and Time Stamps
Uses compile-time macros:
```c
char date[] = __DATE__;  // "Jan 28 2026"
char time[] = __TIME__;  // "14:30:45"
```

### Bill Delimiter
Each bill ends with `~` symbol for search functionality.

## Input Handling

### fgetc(stdin)
Used to clear newline character after scanf:
```c
scanf("%d", &choice);
fgetc(stdin);  // Clear buffer
```

### fgets() for strings
Safer than gets(), prevents buffer overflow:
```c
fgets(name, 100, stdin);
```

### Remove trailing newline
```c
name[strlen(name)-1] = 0;
```

## Search Algorithm

1. Read entire file into string
2. Find customer name position (idx1)
3. Find next '~' symbol after name (idx2)
4. Print characters from idx1-154 to idx2
5. This captures complete bill (154 chars = header size)

## System Commands

### Clear Screen
```c
system("cls");  // Windows
// Use system("clear") for Linux/Mac
```

## Error Handling

- File not found: Program will create new file on first save
- Wrong menu choice: Shows error message
- Invalid input: Basic validation with switch-case default

## Limitations

1. **No input validation** - Doesn't check for negative quantities/prices
2. **Fixed discounts** - 10% and 13% are hardcoded
3. **Basic search** - Searches by exact name match only
4. **No edit/delete** - Can't modify saved invoices
5. **Single file** - All invoices in one file
6. **No database** - Uses text file storage
7. **Platform-specific** - `system("cls")` works on Windows only

## Improvements Possible

**Add:**
- Input validation for quantities and prices
- Option to edit/delete invoices
- Multiple discount rates
- Category-wise items
- Payment method tracking
- Tax exemption option
- Customer phone/address
- Invoice numbering
- Date-based invoice filtering
- Export to PDF
- Database integration
- Cross-platform screen clearing

## File Storage

**Billing.txt** format:
- Append mode (keeps all invoices)
- Each invoice separated by '~'
- Contains complete bill information
- Plain text, easily readable

## Example Session

```
1. Generate Invoice
Enter name of customer: Alice
Enter number of items: 1
For item 1
Enter item name: burger
Enter quantity: 2
Enter price of unit item: 200

[Bill displays]

Do you want to save this bill[y/n]: y
Bill Saved Successfully!!
```

## Sample Bill Calculation

```
Items:
- Burger: 2 × 200 = 400
- Fries: 3 × 100 = 300

Subtotal: 700
Discount @10%: 70
Net Total: 630
VAT @13%: 81.90
Grand Total: 711.90
```

## Compilation Notes

**Standard C libraries used:**
- `stdio.h` - Input/output
- `string.h` - String operations
- `stdlib.h` - System commands

**No special flags needed:**
```bash
gcc billing.c -o billing
```

## Running on Different OS

**Windows:**
- Use `system("cls")`
- Compile with MinGW or Visual Studio

**Linux/Mac:**
- Change `system("cls")` to `system("clear")`
- Compile with gcc

## Data Persistence

All saved invoices remain in `Billing.txt` until:
- File is manually deleted
- File is overwritten
- Data is cleared programmatically

## Use Cases

- Small restaurants
- Food trucks
- Cafeterias
- Canteens
- Small retail shops
- Service providers

## Credits

Built with standard C libraries. Simple, functional billing system for small businesses.

## License

Open source - available for educational purposes.
