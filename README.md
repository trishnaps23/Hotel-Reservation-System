# Hotel-Reservation-System
import datetime

class Room:
    def init(self, room_number, room_type, price, is_ac, is_available=True):
        self.room_number = room_number
        self.room_type = room_type  # Single, Double, Suite
        self.price = price
        self.is_ac = is_ac
        self.is_available = is_available

class Reservation:
    def init(self, customer_name, room, check_in, check_out):
        self.customer_name = customer_name
        self.room = room
        self.check_in = check_in
        self.check_out = check_out

class Hotel:
    def init(self):
        self.rooms = []
        self.reservations = []

    def add_room(self, room_number, room_type, price, is_ac):
        self.rooms.append(Room(room_number, room_type, price, is_ac))

    def view_all_reservations(self):
        print("\nAll Reservations:")
for res in self.reservations:
            print(f"{res.customer_name} - Room {res.room.room_number} ({res.room.room_type}, {'AC' if res.room.is_ac else 'Non-AC'}) "
                  f"from {res.check_in} to {res.check_out}")

    def list_available_rooms(self, room_type=None, is_ac=None):
        print("\nAvailable Rooms:")
        available = False
        for room in self.rooms:
            if room.is_available and (room_type is None or room.room_type.lower() == room_type.lower()) and \
               (is_ac is None or room.is_ac == is_ac):
                print(f"Room {room.room_number} - {room.room_type} - {'AC' if room.is_ac else 'Non-AC'} - ₹{room.price}/night")
                available = True
        if not available:
            print("No available rooms matching criteria.")

    def book_room(self, customer_name, room_type, is_ac, check_in, check_out):
        for room in self.rooms:
            if room.room_type.lower() == room_type.lower() and room.is_ac == is_ac and room.is_available:
                room.is_available = False
                reservation = Reservation(customer_name, room, check_in, check_out)
                self.reservations.append(reservation)
                days = (datetime.datetime.strptime(check_out, "%Y-%m-%d") -
                        datetime.datetime.strptime(check_in, "%Y-%m-%d")).days
                amount = room.price * days
                print(f"\nBooking confirmed for {customer_name} in Room {room.room_number} ({'AC' if is_ac else 'Non-AC'}).")
print(f"Total Amount: ₹{amount}")
                return
        print("No rooms available for selected type and AC preference.")

    def find_reservation(self, customer_name):
        print(f"\nReservations for {customer_name}:")
        found = False
        for res in self.reservations:
            if res.customer_name.lower() == customer_name.lower():
                print(f"Room {res.room.room_number} - {res.room.room_type} ({'AC' if res.room.is_ac else 'Non-AC'}) "
                      f"from {res.check_in} to {res.check_out}")
                found = True
        if not found:
            print("No reservation found.")

    def cancel_reservation(self, customer_name):
        for res in self.reservations:
            if res.customer_name.lower() == customer_name.lower():
                res.room.is_available = True
                self.reservations.remove(res)
                print(f"Reservation for {customer_name} cancelled.")
                return
        print("No reservation found to cancel.")

def main():
    hotel = Hotel()

    # Adding rooms (setup)
    hotel.add_room(101, "Single", 1200, True)
    hotel.add_room(102, "Single", 1000, False)
   hotel.add_room(103, "Double", 1800, True)
    hotel.add_room(104, "Double", 1500, False)
    hotel.add_room(105, "Suite", 3000, True)
    hotel.add_room(106, "Suite", 2700, False)

    while True:
        print("\n--- Hotel Reservation System ---")
        print("1. Customer - Book Room")
        print("2. Customer - View My Reservation")
        print("3. Staff - View All Reservations")
        print("4. Staff - Add Room")
        print("5. Staff - View Available Rooms")
        print("6. Customer - Cancel My Reservation")
        print("7. Exit")
        choice = input("Choose an option: ")

        if choice == "1":
            name = input("Enter your name: ")
            rtype = input("Enter room type (Single/Double/Suite): ").strip()
            ac_input = input("Do you want AC? (yes/no): ").strip().lower()
            is_ac = True if ac_input == "yes" else False
            check_in = input("Enter check-in date (YYYY-MM-DD): ")
            check_out = input("Enter check-out date (YYYY-MM-DD): ")
            hotel.book_room(name, rtype, is_ac, check_in, check_out)

        elif choice == "2":
            name = input("Enter your name: ")
            hotel.find_reservation(name)
elif choice == "3":
            hotel.view_all_reservations()

        elif choice == "4":
            rn = int(input("Enter room number: "))
            rt = input("Enter room type (Single/Double/Suite): ")
            pr = int(input("Enter price per night (in ₹): "))
            ac_input = input("Is it AC? (yes/no): ").strip().lower()
            ac = True if ac_input == "yes" else False
            hotel.add_room(rn, rt, pr, ac)

        elif choice == "5":
            t = input("Enter room type to filter (press Enter for all): ").strip()
            ac_input = input("Filter by AC? (yes/no/skip): ").strip().lower()
            if ac_input == "yes":
                ac_filter = True
            elif ac_input == "no":
                ac_filter = False
            else:
                ac_filter = None
            hotel.list_available_rooms(t if t else None, ac_filter)

        elif choice == "6":
            name = input("Enter your name to cancel reservation: ")
            hotel.cancel_reservation(name)

        elif choice == "7":
            print("Thank you for using the system.")
            break
else:
            print("Invalid choice.")

if name == "main":
    main()
