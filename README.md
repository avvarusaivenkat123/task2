class Transaction:
  def __init__(self, date, amount, category):
    self.date = date
    self.amount = amount
    self.category = category

def load_transactions(filename):
  transactions = []
  with open(filename, 'r') as file:
    for line in file:
      date, amount, category = line.strip().split(',')
      transactions.append(Transaction(date, float(amount), category))
  return transactions

def save_transactions(transactions, filename):
  with open(filename, 'w') as file:
    for transaction in transactions:
      file.write(f"{transaction.date},{transaction.amount},{transaction.category}\n")

def main_menu():
  print("Budget Tracker")
  print("1. Add Expense")
  print("2. Add Income")
  print("3. View Transactions")
  print("4. View Budget")
  print("5. Analyze Expenses")
  print("6. Exit")
  choice = input("Enter your choice: ")
  return choice

def add_transaction(transactions, type):
  amount = float(input(f"Enter {type} amount: "))
  category = input(f"Enter {type} category: ")
  date = input("Enter date (optional, leave blank for today): ") or None
  transactions.append(Transaction(date, amount if type == "Income" else -amount, category))
  save_transactions(transactions, "transactions.csv")
  print(f"{type} added successfully!")

def view_transactions(transactions):
  print("Transactions:")
  for transaction in transactions:
    print(f"{transaction.date} - {transaction.amount} ({transaction.category})")

def calculate_budget(transactions):
  income = sum(t.amount for t in transactions if t.amount > 0)
  expense = sum(t.amount for t in transactions if t.amount < 0)
  remaining = income + expense
  print(f"Total Income: {income:.2f}")
  print(f"Total Expense: {expense:.2f}")
  print(f"Remaining Budget: {remaining:.2f}")

def analyze_expenses(transactions):
  categories = {}
  for transaction in transactions:
    amount = transaction.amount
    category = transaction.category
    categories[category] = categories.get(category, 0) + abs(amount)
  print("Expense Breakdown by Category:")
  for category, total in categories.items():
    print(f"{category}: {total:.2f}")

def main():
  transactions = load_transactions("transactions.csv")
  while True:
    choice = main_menu()
    if choice == '1':
      add_transaction(transactions, "Expense")
    elif choice == '2':
      add_transaction(transactions, "Income")
    elif choice == '3':
      view_transactions(transactions)
    elif choice == '4':
      calculate_budget(transactions)
    elif choice == '5':
      analyze_expenses(transactions)
    elif choice == '6':
      print("Exiting...")
      break
    else:
      print("Invalid choice!")

if __name__ == "__main__":
  main()

