import time
import random

class HotelLevel:
    def __init__(self, level_number, room_capacity_multiplier, staff_skill_increase, room_upgrade_cost, pool_upgrade_cost):
        self.level_number = level_number
        self.room_capacity_multiplier = room_capacity_multiplier
        self.staff_skill_increase = staff_skill_increase
        self.room_upgrade_cost = room_upgrade_cost
        self.pool_upgrade_cost = pool_upgrade_cost

class Room:
    def __init__(self, capacity):
        self.capacity = capacity

class Staff:
    def __init__(self, skill_level):
        self.skill_level = skill_level

class GrandHotelTycoon:
    def __init__(self, name, theme):
        self.name = name
        self.theme = theme
        self.rooms = [Room(1) for _ in range(10)]  # Start with 10 rooms
        self.staff = [Staff(1) for _ in range(5)]  # Start with 5 staff members
        self.customers = 0
        self.levels = {i: HotelLevel(i, 1 + i * 0.1, 5 + i * 2, 50 + i * 20, 500 + i * 50) for i in range(1, 101)}
        self.current_level = 1
        self.money = 250  # Initial budget
        self.parking = False
        self.parking_fee = 50
        self.swimming_pool = False
        self.swimming_fee = 100
        self.theater_entry_fee = 40

    def add_room(self, room):
        room.capacity *= self.levels[self.current_level].room_capacity_multiplier
        self.rooms.append(room)

    def add_staff(self, staff_member):
        staff_member.skill_level += self.levels[self.current_level].staff_skill_increase
        self.staff.append(staff_member)

    def upgrade_room(self):
        if self.money >= self.levels[self.current_level].room_upgrade_cost:
            self.money -= self.levels[self.current_level].room_upgrade_cost
            print(f"Room upgraded to level {self.current_level + 1}!")
            self.levels[self.current_level].room_upgrade_cost *= 2
        else:
            print("Not enough money to upgrade room.")

    def buy_swimming_pool(self):
        if self.current_level >= 10 and not self.swimming_pool and self.money >= 50000:
            self.money -= 50000
            self.swimming_pool = True
            print("Swimming pool added to your Grand Hotel Tycoon!")

    def upgrade_swimming_pool(self):
        if self.swimming_pool and self.money >= self.levels[self.current_level].pool_upgrade_cost and self.current_level <= 100:
            self.money -= self.levels[self.current_level].pool_upgrade_cost
            print(f"Swimming pool upgraded to level {self.current_level}!")

    def collect_swimming_fee(self):
        self.money += len(self.rooms) * self.swimming_fee
        print(f"Swimming fees collected: {len(self.rooms) * self.swimming_fee} Rs")

    def expand(self):
        expansion_cost = 10000 * self.current_level
        if self.money >= expansion_cost:
            self.money -= expansion_cost
            print("Grand Hotel Tycoon expanded! More rooms and amenities available.")
        else:
            print("Not enough money to expand.")

    def add_parking(self):
        if not self.parking and self.current_level >= 5 and self.money >= 250:
            self.money -= 250
            self.parking = True
            print("Parking area added to your Grand Hotel Tycoon!")

    def expand_parking(self):
        expansion_cost = 500 * self.current_level
        if self.parking and self.money >= expansion_cost:
            self.money -= expansion_cost
            print("Parking area expanded!")

    def charge_parking_fee(self):
        if self.parking:
            self.money += len(self.rooms) * self.parking_fee
            print(f"Parking fees collected: {len(self.rooms) * self.parking_fee} Rs")
        else:
            print("No available parking for customers.")

    def collect_swimming_fee(self):
        self.money += len(self.rooms) * self.swimming_fee
        print(f"Swimming fees collected: {len(self.rooms) * self.swimming_fee} Rs")

    def theater_entry(self):
        if self.money >= self.theater_entry_fee:
            self.money -= self.theater_entry_fee
            print("Guests enjoyed a show at the hotel theater!")
        else:
            print("Not enough money for theater entry.")

    def auto_customer_arrival(self):
        customers_arrived = random.randint(1, 10)
        print(f"{customers_arrived} customers have arrived at your Grand Hotel Tycoon!")

        for _ in range(customers_arrived):
            service_choices = random.sample(["Room", "Swimming Pool", "Parking", "Theater"], k=random.randint(1, 4))

            for service_choice in service_choices:
                if service_choice == "Room":
                    if self.rooms:
                        self.collect_swimming_fee()
                    else:
                        print("No available rooms for customers.")

                elif service_choice == "Swimming Pool":
                    if self.swimming_pool:
                        self.collect_swimming_fee()
                        time.sleep(10 * 60)  # 10 minutes for swimming pool activity
                    else:
                        print("No available swimming pool for customers.")

                elif service_choice == "Parking":
                    self.charge_parking_fee()

                elif service_choice == "Theater":
                    self.theater_entry()
                    time.sleep(10 * 60)  # 10 minutes for theater activity

    def upgrade_all_at_level_10(self):
        if self.current_level == 10:
            print("Congratulations! Your Grand Hotel Tycoon has reached Level 10.")
            print("Upgrading all activities to the highest level.")
            self.buy_swimming_pool()
            self.upgrade_swimming_pool()
            self.expand_parking()

    def start_grand_hotel_tycoon(self):
        while True:
            self.upgrade_all_at_level_10()

            print(f"Welcome to {self.name}, a {self.theme} hotel at Level {self.current_level}.")
            print(f"Current budget: {self.money} Rs")
            print(f"{len(self.rooms)} rooms available.")

            self.auto_customer_arrival()

if __name__ == "__main__":
    hotel_tycoon = GrandHotelTycoon(name="Grand Hotel", theme="Modern")
    hotel_tycoon.start_grand_hotel_tycoon()
    
