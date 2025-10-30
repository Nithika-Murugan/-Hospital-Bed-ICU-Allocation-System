# 🏥 Hospital Bed & ICU Allocation System  

A **Java + JavaFX desktop application** for managing **hospital bed and ICU allocations** in real time.  
This project demonstrates **object-oriented programming (OOP)**, clean design patterns, and database integration, making it a practical solution for hospitals to optimize admissions, ICU usage, and waitlist handling.  

---

## ✨ Features
- 👩‍⚕️ **Patient Management** – Register, edit, discharge patients with details (ID, name, age, diagnosis, priority).  
- 🛏️ **Bed Allocation** – Automatic assignment based on **priority + care level** (General, ICU, Isolation).  
- 🔄 **Waitlist Handling** – Patients are queued and automatically promoted when a suitable bed is free.  
- 🩺 **Manual Override** – Clinicians can override allocations, with **audit logging** for traceability.  
- 📊 **Reports** – Export occupancy data & waitlists as **CSV files** or simple text-based reports.  
- 🎨 **JavaFX UI** – Responsive desktop UI with:  
  - Patient tables  
  - Bed occupancy maps (color-coded: 🟢 free, 🔴 occupied)  
  - Charts for ICU/general ward utilization  
- 🗄️ **Database Integration** – Persistent storage using **MySQL + JDBC**.  

---

## 🛠️ Tech Stack
- **Language:** Java 11+  
- **UI:** JavaFX (FXML + CSS styling, charts, tables)  
- **Database:** MySQL (via JDBC)  
- **Design Patterns:** Factory, Observer, Singleton, MVC  
- **Reports:** Apache Commons CSV (CSV export)  
- **Build Tool:** Maven  
- **Testing:** JUnit 5  

---

Reports: Apache Commons CSV (CSV export)
Build Tool: Maven
Testing: JUnit 5
