import json
import os

bookings_file = "appointments.json"

if not os.path.exists(bookings_file):
    with open(bookings_file, "w") as f:
        json.dump([], f)


def load_appointments():
    with open(bookings_file, "r") as f:
        return json.load(f)

def save_appointments(data):
    with open(bookings_file, "w") as f:
        json.dump(data, f, indent=4)

def is_time_available(requested_time):
    appointments = load_appointments()
    for appt in appointments:
        if appt["time"] == requested_time:
            return False
    return True

def book_appointment():
    name = input("Όνομα πελάτη: ")
    service = input("Υπηρεσία (π.χ. 'κούρεμα'): ")
    time = input("Ημερομηνία & Ώρα (π.χ. '2025-07-10 16:00'): ")

    if is_time_available(time):
        new_appt = {
            "name": name,
            "service": service,
            "time": time
        }
        appointments = load_appointments()
        appointments.append(new_appt)
        save_appointments(appointments)
        print(f"\n✅ Το ραντεβού για {name} στις {time} καταχωρήθηκε επιτυχώς!")
    else:
        print("\n❌ Η ώρα αυτή είναι ήδη κλεισμένη. Δοκίμασε άλλη.")

def view_appointments():
    appointments = load_appointments()
    if not appointments:
        print("\n📭 Δεν υπάρχουν ραντεβού.")
        return
    print("\n📅 Λίστα Ραντεβού:")
    for appt in appointments:
        print(f"• {appt['time']} - {appt['name']} ({appt['service']})")

def cancel_appointment():
    time = input("Ποια ώρα θέλεις να ακυρώσεις; (π.χ. '2025-07-10 16:00'): ")
    appointments = load_appointments()
    new_appointments = [a for a in appointments if a["time"] != time]
    if len(new_appointments) != len(appointments):
        save_appointments(new_appointments)
        print("🗑️ Το ραντεβού ακυρώθηκε.")
    else:
        print("⚠️ Δεν βρέθηκε ραντεβού σε αυτή την ώρα.")

def main():
    while True:
        print("\n====== Receptionist Menu ======")
        print("1. Κλείσε Ραντεβού")
        print("2. Προβολή Ραντεβού")
        print("3. Ακύρωση Ραντεβού")
        print("4. Έξοδος")
        choice = input("Επιλογή: ")

        if choice == "1":
            book_appointment()
        elif choice == "2":
            view_appointments()
        elif choice == "3":
            cancel_appointment()
        elif choice == "4":
            print("👋 Κλείσιμο εφαρμογής.")
            break
        else:
            print("❌ Μη έγκυρη επιλογή. Δοκίμασε ξανά.")

if __name__ == "__main__":
    main()
