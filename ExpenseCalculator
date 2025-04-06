import tkinter as tk
from tkinter import ttk, messagebox
import csv
import os

FILENAME = "expense.csv"

if not os.path.exists(FILENAME):
    with open(FILENAME, 'w', newline='') as file:
        writer = csv.writer(file)
        writer.writerow(["Date", "Category", "Amount", "Description"])

# -------------------
# Functions
# -------------------
menu_visible = False

def toggle_menu():
    global menu_visible
    if menu_visible:
        sidebar.place(x=-200, y=0)
        menu_visible = False
    else:
        sidebar.lift()
        sidebar.place(x=0, y=0)
        menu_visible = True

def close_menu():
    global menu_visible
    sidebar.place(x=-200, y=0)
    menu_visible = False

def add_expense():
    date = entry_date.get()
    category = entry_category.get()
    amount = entry_amount.get()
    description = entry_description.get()

    if not (date and category and amount and description):
        messagebox.showwarning("Input Error", "Please fill all fields.")
        return

    try:
        float(amount)
    except ValueError:
        messagebox.showerror("Invalid Input", "Amount must be a number.")
        return

    with open(FILENAME, mode='a', newline='') as file:
        writer = csv.writer(file)
        writer.writerow([date, category, amount, description])

    messagebox.showinfo("Success", "Expense added!")
    entry_date.delete(0, tk.END)
    entry_category.delete(0, tk.END)
    entry_amount.delete(0, tk.END)
    entry_description.delete(0, tk.END)
    view_expenses()

def view_expenses():
    for row in tree.get_children():
        tree.delete(row)

    with open(FILENAME, mode='r') as file:
        reader = csv.reader(file)
        next(reader)
        for row in reader:
            tree.insert('', tk.END, values=row)

def total_expense():
    total = 0
    with open(FILENAME, 'r') as file:
        reader = csv.DictReader(file)
        for row in reader:
            try:
                total += float(row["Amount"])
            except:
                pass
    messagebox.showinfo("Total Expense", f"Total: ₹{total:.2f}")

# -------------------
# GUI Window
# -------------------
root = tk.Tk()
root.title("💸 Expense Tracker")
root.geometry("480x800")
root.configure(bg="#2e2e2e")  # Dark gray
root.resizable(False, False)

font_mono = ("Courier New", 10)

# -------------------
# Sidebar
# -------------------
sidebar = tk.Frame(root, width=200, height=800, bg="#1e1e1e")
sidebar.place(x=-200, y=0)

tk.Label(sidebar, text="MENU", font=("Courier New", 14, "bold"), bg="#1e1e1e", fg="white").pack(pady=20)

tk.Button(sidebar, text="Add Expense", font=font_mono, command=add_expense, bg="yellow", fg="black", relief="flat").pack(fill="x", pady=5, padx=10)
tk.Button(sidebar, text="View Expenses", font=font_mono, command=view_expenses, bg="yellow", fg="black", relief="flat").pack(fill="x", pady=5, padx=10)
tk.Button(sidebar, text="Total Expense", font=font_mono, command=total_expense, bg="yellow", fg="black", relief="flat").pack(fill="x", pady=5, padx=10)
tk.Button(sidebar, text="Exit", font=font_mono, command=root.quit, bg="red", fg="white", relief="flat").pack(fill="x", pady=10, padx=10)
tk.Button(sidebar, text="Close Menu ✖", font=font_mono, command=close_menu, bg="#555", fg="white", relief="flat").pack(fill="x", pady=40, padx=10)

# -------------------
# Menu Toggle Button
# -------------------
toggle_btn = tk.Button(root, text="☰", font=("Courier New", 16), bg="#2e2e2e", fg="white", bd=0, command=toggle_menu)
toggle_btn.place(x=10, y=10)

# -------------------
# Main Content Frame
# -------------------
main_frame = tk.Frame(root, bg="#2e2e2e")
main_frame.place(x=20, y=50, width=440, height=730)

form_frame = tk.LabelFrame(main_frame, text="Add Expense", font=("Courier New", 11, "bold"), bg="#2e2e2e", fg="white", padx=10, pady=5)
form_frame.pack(fill="x", pady=10)

tk.Label(form_frame, text="Date (YYYY-MM-DD)", bg="#2e2e2e", fg="white", font=font_mono).grid(row=0, column=0, sticky="w", pady=5)
entry_date = tk.Entry(form_frame, width=28)
entry_date.grid(row=0, column=1, pady=5)

tk.Label(form_frame, text="Category", bg="#2e2e2e", fg="white", font=font_mono).grid(row=1, column=0, sticky="w", pady=5)
entry_category = tk.Entry(form_frame, width=28)
entry_category.grid(row=1, column=1, pady=5)

tk.Label(form_frame, text="Amount (₹)", bg="#2e2e2e", fg="white", font=font_mono).grid(row=2, column=0, sticky="w", pady=5)
entry_amount = tk.Entry(form_frame, width=28)
entry_amount.grid(row=2, column=1, pady=5)

tk.Label(form_frame, text="Description", bg="#2e2e2e", fg="white", font=font_mono).grid(row=3, column=0, sticky="w", pady=5)
entry_description = tk.Entry(form_frame, width=28)
entry_description.grid(row=3, column=1, pady=5)

# -------------------
# Expense Table
# -------------------
table_frame = tk.LabelFrame(main_frame, text="Records", bg="#2e2e2e", fg="white", font=("Courier New", 11, "bold"))
table_frame.pack(fill="both", expand=True, pady=10)

columns = ("Date", "Category", "Amount", "Description")
tree = ttk.Treeview(table_frame, columns=columns, show="headings", style="Custom.Treeview")

style = ttk.Style()
style.theme_use("default")
style.configure("Custom.Treeview", background="#2e2e2e", foreground="white", fieldbackground="#2e2e2e", rowheight=30, font=font_mono)
style.map("Custom.Treeview", background=[("selected", "#444")])

for col in columns:
    tree.heading(col, text=col)
    tree.column(col, anchor="center", width=100)

vsb = ttk.Scrollbar(table_frame, orient="vertical", command=tree.yview)
tree.configure(yscroll=vsb.set)
vsb.pack(side='right', fill='y')
tree.pack(fill="both", expand=True)

view_expenses()
root.mainloop()
