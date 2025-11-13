# Disk Volumes — Study Notes

---

## What are disk volumes? (1 of 3)

### Simple volumes / partitions
- Reside on **one disk**.
- Can be extended into contiguous or noncontiguous space.
  - If you try to extend a partition on a **basic disk** into noncontiguous space, Windows will prompt you to convert the disk to **dynamic**.

### Spanned volumes (require dynamic disks)
- A spanned volume uses **multiple disks**.
- Displayed in Windows File Explorer as **one single volume**.
- Not fault tolerant:
  - If any disk in the spanned volume fails, **all data is lost**.

---

## What are disk volumes? (2 of 3)

### Striped volumes (dynamic disks required)
- Data is **striped across two or more disks** (up to 32).
- Provide excellent **read and write performance**.
- **Not fault tolerant**.
- Cannot be **extended or shrunk**.

### Mirrored volumes
- Data is mirrored between **two disks**.
- Provides **fault tolerance**:
  - If one disk fails, the system can still access the mirror.
- Cannot be **extended or shrunk**.

---

## What are disk volumes? (3 of 3)

### RAID-5 volume
- Requires **three or more disks** (up to 32).
- Provides **fault tolerance**:
  - If one disk fails, the volume remains accessible.
- Excellent **read performance**, good **write performance**.
- Cannot be **extended, shrunk, or mirrored**.
- RAID stands for **Redundant Array of Independent Disks**.

---

# Options for Managing Volumes (1 of 3)

## Accessing tools from Server Manager

1. In the **navigation pane**, select **File and Storage Services**, then choose **Volumes**.
2. Right-click a volume to open the context menu, then choose:
   - **Extend volume**
   - **Delete volume**
   - **Format**

## Creating a volume

1. Select the **Tasks** drop-down list and choose **New Volume**.
2. Use the **New Volume Wizard** to configure:
   - Volume size  
   - Drive letter  
   - File system  

---

# Options for Managing Volumes (2 of 3)

## Disk Management overview
- Lists all available disks and the amount of **unallocated space** on each disk.

## Creating a new volume in Disk Management

1. Right-click **unallocated space** → select the volume type to create.
2. Use the wizard to set:
   - Size  
   - Drive letter  
   - File system
3. Choose whether to **Perform Quick Format**:
   - Quick format does **not** check for read/write errors.

---

# Options for Managing Volumes (3 of 3)

## Diskpart

### Why Diskpart?
- Useful for **Server Core** installations.
- Helpful when creating **scripts**.

### Accessing Diskpart

1. At a command prompt, type:  
   **Diskpart**
2. At the Diskpart prompt, enter commands:
   - **list disk** → displays all disks.
   - **select disk X** → chooses the disk to work with.

# Extend and Shrink Volumes — Study Notes

---

## Extend and shrink volumes (1 of 3)

- On **dynamic disks**, you can extend a volume into **contiguous or noncontiguous** space.
- On **basic disks**, you cannot extend into noncontiguous space.
- Volumes must be formatted with **NTFS** or **ReFS** to be extended.
- Extending a volume is **nondestructive**, meaning no data is lost.

---

## Extend and shrink volumes (2 of 3)

### Spanned disks
You can create a **spanned disk** by extending a volume onto another disk.

Important points:

- Can include **2 to 32 disks**.
- Spanned disks are **not fault tolerant**.
- Useful when one disk runs out of space.
- All disks must be **dynamic disks**.
- Spanned volumes appear in **different colors** in Disk Management.

---

## Extend and shrink volumes (3 of 3)

### Shrinking volumes
- Only volumes formatted with **NTFS** can be shrunk.

### How shrinking works
- Shrinking reclaims unused space from the **end of the volume**.
- The shrink process works backward until it encounters a **file or file fragment**.
  - These fragments can limit how much you can shrink.

### Immovable files
Some files are **immovable**, such as the Windows paging file, which prevents shrinking.

To resolve:
- Configure Windows to **relocate immovable files**.

### Fragmentation considerations
A fragmented volume may have file fragments at the end.

To improve shrink capability:
- Run **Windows Defragment and Optimize Drives**.

# RAID — Study Notes

---

## What is RAID?

**RAID (Redundant Array of Independent Disks)** is a way of combining **multiple physical disks** so they behave like **one logical disk**.

Goals:
- Improve **performance** (faster reads/writes)
- Improve **fault tolerance** (survive a disk failure)
- Or **both**, depending on the RAID level

Think of RAID like a **team of workers**:
- Some teams are focused on speed.
- Some teams focus on backup and reliability.
- Some teams try to balance both.

---

## Hardware RAID vs. Software RAID

### Hardware RAID

Implemented by a **RAID controller card** (or RAID-capable motherboard).

Key points:
- Configured using **vendor tools** (BIOS menus, web UI, etc.).
- Often supports **hot swapping** (replace a disk while the system is running).
- The **OS just sees one disk**, even though multiple disks are behind it.
- Usually gives better **performance** and **reliability**.

**Real-world analogy:**  
A dedicated **supervisor** manages several workers. The supervisor decides who does what and keeps things running smoothly. The company (OS) just sees “one team” and doesn’t care how it’s organized internally.

---

### Software RAID (OS-based RAID)

Implemented by the **operating system** (for example, Windows using dynamic disks).

Key points:
- Uses **normal disks**; no special RAID card required.
- Very **flexible** and **cheap**.
- Performance depends on the **CPU and OS**.
- If the OS is corrupted, RAID management and recovery can be harder.

**Real-world analogy:**  
The workers manage themselves with no supervisor. It works and is cheaper, but if the team leader (OS) gets sick, coordination can fall apart.

---

## RAID Levels Overview (Concepts)

Windows Server (using dynamic disks) supports these main software RAID levels:

- **RAID-0** → striped volume (performance)
- **RAID-1** → mirrored volume (fault tolerance)
- **RAID-5** → RAID-5 volume (performance + fault tolerance)

---

## RAID-0 (Striping)

**Goal:** Maximum performance.  
**Disks required:** Minimum 2, up to 32 (in Windows).

### How it works

Data is **split into chunks** and spread across all disks.

Example write:
- Chunk 1 → Disk 1  
- Chunk 2 → Disk 2  
- Chunk 3 → Disk 1  
- Chunk 4 → Disk 2 …

Simple diagram:

    RAID-0 (no redundancy)
    +-----------------------+
    | Disk 1 | A1 | A3 |...|
    | Disk 2 | A2 | A4 |...|
    +-----------------------+

Because reads/writes happen in parallel on multiple disks, performance is very good.

### Pros
- **Very fast** read and write performance.
- No space is “wasted” on redundancy.

### Cons
- **No fault tolerance**.  
  If **one disk fails**, the **entire array is lost**, because each file is split across multiple disks.

### Real-world example
Imagine a book where:
- Odd pages are stored on Desk 1.
- Even pages are stored on Desk 2.

Reading is fast because two people can hand you pages at once.  
But if Desk 2 collapses, you’re missing half the pages → the whole book is useless.

---

## RAID-1 (Mirroring)

**Goal:** Fault tolerance (protect against disk failure).  
**Disks required:** Exactly 2 (for classic RAID-1 in Windows).

### How it works

Each disk stores **an exact copy** of the same data.

Diagram:

    RAID-1 (mirroring)
    +---------------------+
    | Disk 1 |   DATA A   |
    | Disk 2 |   DATA A   |
    +---------------------+

When you write data, it’s written to **both disks**.  
If either disk fails, the other still has a full copy.

### Pros
- **Excellent fault tolerance** — you can lose one disk and keep running.
- Read performance can be slightly better (reads can come from either disk).

### Cons
- You “lose” 50% of your raw capacity:
  - 2 × 1 TB disks ⇒ 1 TB usable RAID-1 volume.
- No performance boost for writes (must write twice).

### Real-world example
Two identical photo albums:
- You keep one at home, one at a friend’s house.
- If one is destroyed, you still have all the photos.

**Common use:** Protecting the **OS drive** so the server keeps running even if one disk dies.

---

## RAID-5 (Striping with Parity)

**Goal:** Balance between **performance** and **fault tolerance**.  
**Disks required:** Minimum 3, up to 32 (in Windows).

### How it works

Data is striped across disks **like RAID-0**, but RAID-5 also stores **parity information**.  
Parity is like a math checksum that lets the system **recreate data** if one disk fails.

Simplified layout:

    Disk 1:  A1   B1   P1
    Disk 2:  A2   P2   C1
    Disk 3:  P3   B2   C2

- A1 + A2 + P? → enough information to rebuild a missing piece.
- Parity (P) is spread across disks so no single disk is “parity-only”.

### Pros
- **Excellent read performance**, good write performance.
- Survives **one disk failure** with no data loss.

### Cons
- More complex than RAID-1.
- **Rebuilds are slow and stressful** on remaining disks.
- If **two disks fail**, the array is lost.

### Real-world example
Three friends share notes:
- Two store actual notes.
- The third stores a “formula” (parity) that can recreate a friend’s notes if they lose them.

As long as **only one friend loses their notes**, the group can restore everything.

---

## RAID Levels — Quick Comparison

| RAID Level | Disks (Min–Max) | Main Goal              | Performance Benefit?                 | Fault Tolerance?                    |
|-----------:|-----------------|------------------------|--------------------------------------|-------------------------------------|
| **RAID-0** | 2–32            | Speed                  | ✅ Very good read & write            | ❌ None                             |
| **RAID-1** | 2–2             | Redundancy             | ❌ No (writes same data twice)       | ✅ Survives 1 disk failure          |
| **RAID-5** | 3–32            | Speed + redundancy     | ✅ Excellent read, good write        | ✅ Survives 1 disk failure          |

---

## RAID Levels in Windows Disk Management (How to create them)

**Prerequisites:**
- All disks must be configured as **dynamic disks**.
- You use the **Disk Management** console.

### Creating RAID-0 (Striped Volume)

1. In Disk Management, right-click **unallocated space**.
2. Choose **New Striped Volume**.
3. Select at least **two disks**, assign a drive letter, choose a file system, and format.

### Creating RAID-1 (Mirrored Volume)

You have two options:

- **Mirror an existing volume**
  1. Right-click an existing **simple volume**.
  2. Select **Add Mirror**.
  3. Choose the second disk.

- **Create a new mirrored volume**
  1. Right-click **unallocated space**.
  2. Select **New Mirrored Volume**.
  3. Choose two disks, assign a drive letter, and format.

### Creating RAID-5 (RAID-5 Volume)

1. In Disk Management, right-click **unallocated space**.
2. Select **New RAID-5 Volume**.
3. Choose at least **three disks**, assign a drive letter, and format.

---

## Which RAID should I choose in the real world?

- **RAID-0**  
  - Use when: You need speed and don’t care about losing the data  
    (e.g., temp scratch disk, non-critical video editing workspace).

- **RAID-1**  
  - Use when: You want **simple, strong protection** for important data or the OS.  
    (e.g., system volume on a small server).

- **RAID-5**  
  - Use when: You want a good balance of **capacity, speed, and fault tolerance**  
    (e.g., general file storage on servers).


