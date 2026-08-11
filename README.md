# Contact-book
Contact Book (GUI) is a Python-based graphical application developed using Tkinter. It allows users to add, search, view, and delete contact information such as names, phone numbers, and email addresses.
import tkinter as tk
from tkinter import messagebox

contacts = []


def add_contact():
    name = name_entry.get()
    phone = phone_entry.get()
    email = email_entry.get()

    if name == "" or phone == "":
        messagebox.showwarning("Warning", "Name and Phone are required!")
        return

    contacts.append({
        "name": name,
        "phone": phone,
        "email": email
    })

    messagebox.showinfo("Success", "Contact added successfully!")
    clear_fields()
    display_contacts()


def delete_contact():
    selected = contact_list.curselection()

    if not selected:
        messagebox.showwarning("Warning", "Please select a contact!")
        return

    index = selected[0]
    contacts.pop(index)

    display_contacts()
    messagebox.showinfo("Success", "Contact deleted successfully!")


def search_contact():
    search_text = search_entry.get().lower()

    contact_list.delete(0, tk.END)

    for contact in contacts:
        if search_text in contact["name"].lower():
            contact_list.insert(
                tk.END,
                f'{contact["name"]} | {contact["phone"]} | {contact["email"]}'
            )


def display_contacts():
    contact_list.delete(0, tk.END)

    for contact in contacts:
        contact_list.insert(
            tk.END,
            f'{contact["name"]} | {contact["phone"]} | {contact["email"]}'
        )


def clear_fields():
    name_entry.delete(0, tk.END)
    phone_entry.delete(0, tk.END)
    email_entry.delete(0, tk.END)


# Main window
root = tk.Tk()
root.title("Contact Book")
root.geometry("650x500")

# Title
title = tk.Label(
    root,
    text="Contact Book",
    font=("Arial", 22, "bold")
)
title.pack(pady=15)

# Input frame
input_frame = tk.Frame(root)
input_frame.pack(pady=10)

tk.Label(input_frame, text="Name").grid(row=0, column=0, padx=10, pady=5)
name_entry = tk.Entry(input_frame, width=30)
name_entry.grid(row=0, column=1)

tk.Label(input_frame, text="Phone").grid(row=1, column=0, padx=10, pady=5)
phone_entry = tk.Entry(input_frame, width=30)
phone_entry.grid(row=1, column=1)

tk.Label(input_frame, text="Email").grid(row=2, column=0, padx=10, pady=5)
email_entry = tk.Entry(input_frame, width=30)
email_entry.grid(row=2, column=1)

# Add button
tk.Button(
    root,
    text="Add Contact",
    command=add_contact,
    width=20
).pack(pady=5)

# Search
search_frame = tk.Frame(root)
search_frame.pack(pady=10)

tk.Label(search_frame, text="Search Name").pack(side=tk.LEFT)

search_entry = tk.Entry(search_frame, width=25)
search_entry.pack(side=tk.LEFT, padx=5)

tk.Button(
    search_frame,
    text="Search",
    command=search_contact
).pack(side=tk.LEFT)

# Contact list
contact_list = tk.Listbox(
    root,
    width=75,
    height=12
)
contact_list.pack(pady=10)

# Delete button
tk.Button(
    root,
    text="Delete Selected",
    command=delete_contact,
    width=20
).pack(pady=5)

root.mainloop()
