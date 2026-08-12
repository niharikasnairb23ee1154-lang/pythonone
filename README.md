import csv
import os
from collections import defaultdict
from datetime import datetime

FILE_NAME = "expenses.csv"


# Create CSV file with headers if it doesn't exist
def initialize_file():
    if not os.path.exists(FILE_NAME):
        with open(FILE_NAME, "w", newline="") as file:
            writer = csv.writer(file)
            writer.writerow(["Date", "Category", "Amount", "Note"])


# Add a new expense
def add_expense():
    try:
        date = input("Enter Date (YYYY-MM-DD): ")

      
        datetime.strptime(date, "%Y-%m-%d")

        category = input("Enter Category: ").strip()

        amount = float(input("Enter Amount: "))
        if amount <= 0:
            raise ValueError("Amount must be greater than zero.")

        note = input("Enter Note (Optional): ")

        with open(FILE_NAME, "a", newline="") as file:
            writer = csv.writer(file)
            writer.writerow([date, category, amount, note])

        print("Expense added successfully!")

    except ValueError as e:
        print("Invalid Input:", e)
    except Exception as e:
        print("Error:", e)


# View all expenses
def view_expenses():
    try:
        total = 0

        with open(FILE_NAME, "r") as file:
            reader = csv.reader(file)
            next(reader)  # Skip header

            print("\n---------------- Expense Records ----------------")
            print("{:<12} {:<15} {:<10} {}".format("Date", "Category", "Amount", "Note"))
            print("-" * 55)

            found = False
            for row in reader:
                found = True
                print("{:<12} {:<15} {:<10} {}".format(row[0], row[1], row[2], row[3]))
                total += float(row[2])

            if not found:
                print("No expense records found.")

            print("-" * 55)
            print(f"Total Amount Spent: ₹{total:.2f}")

    except FileNotFoundError:
        print("Expense file not found.")
    except Exception as e:
        print("Error:", e)


# Display category-wise summary
def category_summary():
    try:
        summary = defaultdict(float)

        with open(FILE_NAME, "r") as file:
            reader = csv.reader(file)
            next(reader)

            for row in reader:
                summary[row[1]] += float(row[2])

        if not summary:
            print("No expense records found.")
            return

        print("\n------- Category-wise Spending Summary -------")
        for category, amount in summary.items():
            print(f"{category:<20} ₹{amount:.2f}")

    except FileNotFoundError:
        print("Expense file not found.")
    except Exception as e:
        print("Error:", e)


# Main menu
def menu():
    initialize_file()

    while True:
        print("\n====== Expense Tracker ======")
        print("1. Add Expense")
        print("2. View All Expenses")
        print("3. Category-wise Summary")
        print("4. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            add_expense()
        elif choice == "2":
            view_expenses()
        elif choice == "3":
            category_summary()
        elif choice == "4":
            print("Thank you for using Expense Tracker!")
            break
        else:
            print("Invalid choice! Please try again.")


# Run the program
menu()
