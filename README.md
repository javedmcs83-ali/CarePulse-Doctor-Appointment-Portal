# 🩺 CarePulse: Doctor Appointment Booking Portal

A **Power Apps Canvas app on Microsoft Dataverse** that digitizes clinic appointment booking from first call to consultation. Receptionists, Doctors and Clinic Admins each land on their own screen at launch, and a **Power Automate** flow emails the patient a branded booking card the moment an appointment is created.

![Power Apps](https://img.shields.io/badge/Power%20Apps-Canvas-742774) ![Dataverse](https://img.shields.io/badge/Microsoft-Dataverse-0078D4) ![Power Automate](https://img.shields.io/badge/Power%20Automate-Cloud%20Flow-0B6A7E) ![Solutions](https://img.shields.io/badge/ALM-Solutions-5B21B6)

> 🔗 **Live project page:** https://javedmcs83-ali.github.io/REPO-NAME/ *(replace `REPO-NAME` after enabling GitHub Pages)*

## 🎯 Problem
- Appointments were noted from calls and walk-ins in separate sheets, with no single source of truth
- Reception, doctors and admins needed different views of the same data
- Patients got no written confirmation of date, slot, doctor and appointment ID
- Doctor specialty, OPD room and fee were not available where bookings were made

## 👥 Roles
| Role | Start screen | What they do |
|---|---|---|
| Receptionist | `Scr_Patient_Bookings` | Search, create and edit appointments; print confirmation card |
| Doctor | `Scr_Doctor_Consulation` | View appointments by date, update status, record notes |
| Clinic Admin | `Scr_Clinic_Admin` | Manage Doctors Directory; jump to bookings and consultations |
| Patient | (no app access) | Receives the confirmation email |

## 🗂️ Dataverse tables
- **Tbl_User_Roles**: maps user email to role (drives start screen)
- **Tbl_Appointment**: 11 columns; patient, specialty, doctor, date, time slot, reason, status, consultation notes
- **Tbl_Doctors_Directory**: 5 columns; doctor ID, name, specialty, fee, OPD room

![User roles table](screenshots/01-user-roles-table.png)

## 🔐 Role-based start screen routing
```powerfx
// App.Formulas
Var_Logged_User_Role = LookUp(
    Tbl_User_Roles,
    Lower('User Email Address') = Lower(User().Email)
).'User Role';

// App.StartScreen
Switch(Var_Logged_User_Role,
    "Receptionist", Scr_Patient_Bookings,
    "Doctor", Scr_Doctor_Consulation,
    "Clinic Admin", Scr_Clinic_Admin
)
```

## 📱 Canvas screens
> Screenshots use sample test data from a personal Microsoft tenant. Names and phone numbers are not real; email addresses are blurred.

**1. Clinic Admin: Doctors Directory**
![Clinic Admin](screenshots/02-clinic-admin-screen.png)

**2. Patient Bookings: gallery and form**
![Bookings gallery](screenshots/03-patient-bookings-gallery.png)
![Bookings form](screenshots/04-patient-bookings-form.png)

**3. Appointment Confirmation: printable HTML card**
![Confirmation card](screenshots/05-appointment-confirmation-card.png)

**4. Doctor Consultation**
![Doctor consultation](screenshots/06-doctor-consultation-screen.png)

## ⚡ Power Automate: "CarePulse - Appointment Created"
Dataverse *row added* trigger → **TimeSlotMap** compose (choice number to readable time) → **Send an email (V2)** with an HTML card to the patient.

| Trigger | TimeSlotMap |
|---|---|
| ![Trigger](screenshots/07-flow-trigger.png) | ![TimeSlotMap](screenshots/08-flow-timeslotmap.png) |

![Email part 1](screenshots/09-flow-email-part1.png) ![Email part 2](screenshots/10-flow-email-part2.png)

**Email received by the patient in Outlook**
![Patient email](screenshots/11-patient-email-outlook.png)

## 🛠️ Tech stack
Power Apps Canvas · Microsoft Dataverse · Power Automate · Office 365 Outlook · Solutions · Power Fx

## 📈 Delivered
3 Dataverse tables · 4 canvas screens · 3 roles with automatic routing · 5 bookable time slots · 4 appointment statuses · 1 automated confirmation-email flow

## 👤 Author
**Javed Pinjari**: Power Platform Developer · [LinkedIn](https://www.linkedin.com/in/javed-pinjari-0a176582/) · [GitHub](https://github.com/javedmcs83-ali)

*Personal learning project using synthetic data. Not affiliated with or endorsed by any employer or client.*
