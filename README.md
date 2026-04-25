# Lost-and-Found-Item-Tracker
The Lost &amp; Found Item Tracker is a simple digital system that helps students and staff record lost and found items at school. It organizes item descriptions and locations, making it easier to match lost items with found ones and return them to their owners.

# Lost & Found Tracker

A Python desktop application for managing lost and found items, built with a Tkinter GUI and a Flask REST API backend. Data is persisted locally using CSV files.

---

## Requirements

- Python 3.8+
- The following Python packages:
  - `pandas`
  - `flask`
  - `requests`
  - `tkinter` *(included with most Python installations)*

Install dependencies with:

```bash
pip install pandas flask requests (tkinter) if not installed yet

```

---

## Getting Started

1. Clone or download the "Lost-and-Found-Item-Tracker" file included in the repository into visual studio code.
2. Open a terminal in the project folder.
3. Run the application:

```bash
python main.py
```

This will:
- Initialize the CSV data files (`lost_items.csv` and `found_items.csv`) if they don't already exist.
- Start the Flask REST API in the background on port `5000`.
- Open the desktop GUI window.

---
## Video Demo: 
https://youtu.be/zmypEVo1YsI?si=n2Otp7QVnlemypt7

## Using the Application

When launched, the main window presents the following buttons:

### Register Lost Item
Opens a series of dialog boxes asking for:
- **Description** – What the item is.
- **Location** – Where it was lost.
- **Date** – Defaults to today's date (fetched from an online API or falls back to system date).
- **Student ID (Matrícula)** – The student ID of the person who lost the item.

The item is saved to `lost_items.csv`.

---

### Register Found Item
Opens a series of dialog boxes asking for:
- **Description** – What the item is.
- **Location** – Where it was found.
- **Date** – Defaults to today's date.

The item is saved to `found_items.csv`.

---

### View Items
Opens a new window displaying two tables:
- **Lost Items** – All registered lost items with description, location, date, and student ID.
- **Found Items** – All registered found items with description, location, and date.

---

### Match Items
Compares descriptions of all lost and found items to find possible matches. A match is detected when one description is contained within the other (case-insensitive). Results are displayed in a table showing the matched lost and found item descriptions side by side.

---

### Delete Items
Opens a tabbed window with two tabs:
- **Delete Lost Items** – Select a lost item from the table and click *Delete Selected* to remove it.
- **Delete Found Items** – Select a found item from the table and click *Delete Selected* to remove it.

A confirmation dialog is shown before any item is permanently deleted.

---

### Exit
Closes the application.

---

## Data Storage

All data is stored locally in two CSV files created automatically on first run:

| File | Contents |
|------|----------|
| `lost_items.csv` | Description, location, date, student ID (matrícula) |
| `found_items.csv` | Description, location, date |

---

## REST API

The application also exposes a Flask REST API on `http://localhost:5000`. This runs automatically in the background when the program starts.

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/lost` | Returns all lost items |
| `POST` | `/lost` | Adds a new lost item (requires `description`, `location`, `date`, `matricula`) |
| `DELETE` | `/lost/<id>` | Deletes a lost item by index |
| `GET` | `/found` | Returns all found items |
| `POST` | `/found` | Adds a new found item (requires `description`, `location`, `date`) |
| `DELETE` | `/found/<id>` | Deletes a found item by index |
| `GET` | `/matches` | Returns possible matches between lost and found items |

### Example API Usage

```bash
# Get all lost items
curl http://localhost:5000/lost

# Register a new found item
curl -X POST http://localhost:5000/found \
  -H "Content-Type: application/json" \
  -d '{"description": "blue backpack", "location": "Library", "date": "2024-11-01"}'

# Get possible matches
curl http://localhost:5000/matches
```

---

## Notes

The initial impact of this project is positive, as it provides a simple yet functional solution to a common problem in schools: lost personal items. Even though it is a basic system, it improves the organization of lost and found processes and reduces the time students spend trying to recover their belongings.

Furthermore, this project has strong potential for future development. It could be expanded into a more advanced application with a graphical user interface, automatic notifications, or integration with an online database. In its current form, it demonstrates how a simple technological tool can create a meaningful and practical impact in everyday student life.



- The date field auto-fetches from the [World Time API](http://worldtimeapi.org) (America/New_York timezone). If the API is unavailable, the system date is used instead.
- The matching algorithm is basic substring matching. An item is considered a possible match if one description contains the other.
- Deleting an item resets the internal row indices, so IDs shown in the UI may change after a deletion.
