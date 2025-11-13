# PowerShell Arrays & Hash Tables — Study Notes

---

# 1. What Is an Array?

An **array** is a variable that holds **multiple values** instead of just one.

Think of an array like:
- A **row of mailboxes** (each box holds one item)
- A **shopping list** (multiple items in one place)
- A **folder with several papers** inside it

Use arrays when you want to work with a **group** of items:
- Multiple computer names  
- A list of AD users  
- A set of stopped services  
- A list of numbers  

---

## Creating Arrays

### Comma-separated values:
`$computers = "LON-DC1","LON-SRV1","LON-SRV2"`  
`$numbers = 228,43,102`

Important:
- Quotes around each item = separate strings  
- One set of quotes around everything = **one single string**

### Create an array from a command:
`$users = Get-ADUser -Filter *`  
`$files = Get-ChildItem C:\`

### Check if a variable is an array:
`$computers.GetType()`

### Create an empty array:
`$newUsers = @()`

### Force a variable to become an array:
`[array]$computers = "LON-DC1"`

---

# 2. Working With Arrays

## Display all items  
`$computers`

## Access individual items (index numbers)

Arrays start at **index 0**:
- Item 0 = first  
- Item 1 = second  
- Item 2 = third  

Example:
`$users[0]`

## Add items to an array

PowerShell recreates arrays every time you add to them, so performance can slow with *huge* arrays. But conceptually:

Standard add:
`$users = $users + $user1`

Shortcut add:
`$users += $user1`

## Explore what array items can do

Send array items into `Get-Member`:
`$files | Get-Member`

If the array contains mixed types, you will see member info for **each** type.

## Explore the array object itself

`Get-Member -InputObject $files`

---

# 3. Fixed-Size Arrays vs Array Lists

### Important:
Normal PowerShell arrays are **fixed-size**.  
This means:
- Adding an item recreates the array  
- Removing an item is inconvenient

If you're adding **lots** of items, use an **ArrayList** instead.

---

# 4. Array Lists

ArrayLists behave like arrays BUT:
- They are **not fixed-size**
- They support **Add()** and **Remove()** methods
- They perform better when adding/removing lots of items

## Create an array list with starting values:
`[System.Collections.ArrayList]$computers = "LON-DC1","LON-SVR1","LON-CL1"`

## Create an empty array list:
`$computers = New-Object System.Collections.ArrayList`

## Add items:
`$computers.Add("LON-SRV2")`

## Remove items:
`$computers.Remove("LON-CL1")`  
(Removes only the **first matching** entry)

## Remove by index:
`$computers.RemoveAt(1)`

---

# 5. What Is a Hash Table?

A **hash table** is like an array, but instead of using number indexes (0,1,2…),  
you use **unique keys** (like labels).

Think of it like:
- A **dictionary** (word → definition)
- A **phone book** (name → phone number)
- A **locker room** (locker number → contents)

Each key points to a value.

### Example: Array of IP addresses

| Index | Value        |
|-------|--------------|
| 0     | 172.16.0.10  |
| 1     | 172.16.0.11  |
| 2     | 172.16.0.40  |

Access:
`$ip[0]`

### Hash table version:

| Key      | Value         |
|----------|---------------|
| LON-DC1  | 192.168.0.10  |
| LON-SRV1 | 192.168.0.11  |
| LON-SRV2 | 192.168.0.12  |

Access:
`$servers['LON-DC1']`  
or  
`$servers.'LON-DC1'`

Keys with special characters (like hyphens) must be quoted.

---

# 6. Creating Hash Tables

Basic creation:
`$servers = @{"LON-DC1"="172.16.0.10"; "LON-SRV1"="172.16.0.11"}`

Syntax rules:
- Starts with `@`
- Keys + values inside `{ }`
- Items separated by semicolons (only needed when all items are on one line)

### Multi-line version (no semicolons needed):
```powershell
$servers = @{
"LON-DC1" = "172.16.0.10"
"LON-SRV1" = "172.16.0.11"
}
```

---

# 7. Working With Hash Tables

## Add a new entry:
`$servers.Add("LON-SRV2","172.16.0.12")`

## Remove an entry:
`$servers.Remove("LON-DC1")`

## Update an existing entry:
`$servers."LON-SRV2" = "172.16.0.100"`

## Inspect the hash table:
`$servers | Get-Member`

---

# Real-World Examples

### Create a list of server names for a script:
`$servers = "DC1","SRV1","SRV2","SRV3"`

### Restart a group of services:
```powershell
$services = "BITS","W32Time","Dnscache"
foreach ($s in $services) { Restart-Service $s }
```

### Store servers + their IP addresses:
```powershell
$servers = @{
"DC1" = "192.168.0.10"
"SRV1" = "192.168.0.20"
"SRV2" = "192.168.0.30"
}
```

### Look up a server’s IP:
`$servers['SRV1']`

---

# Summary (What You Should Know)

- Arrays store **multiple values** (index-based).
- ArrayLists are **flexible** arrays that can easily grow/shrink.
- Hash tables store **key/value pairs** (like dictionaries).
- Arrays start at **index 0**.
- ArrayLists support `.Add()` and `.Remove()`.
- Hash tables use **keys**, not index numbers.
- Use `Get-Member` to explore what an array or hash table can do.

# Module Assessment — Arrays & Hash Tables

---

## 1. How do you refer to the FIRST item in an array?

Correct answer:  
`$users[0]`

Explanation:  
Arrays start at **index 0**, so the first item is always `[0]`.

---

## 2. Which command updates the value for the key **dc1** in a hash table?

Correct answer:  
`$computers.dc1 = "Domain Controller"`

Explanation:  
To update a hash table entry, use:

- `$hashTable.key = "value"`  
  or  
- `$hashTable["key"] = "value"`

Both work, but **dot notation** is the cleanest when the key has no spaces/special characters.

