# JCDSOL15-CAPSTONE1-RMGT
Yellow Pages - Contact Management Application
Overview
Aplikasi ini adalah sebuah sistem manajemen kontak sederhana yang dirancang menggunakan bahasa pemrograman Python. Tujuan utamanya adalah untuk memudahkan pengguna dalam melakukan operasi dasar CRUD (Create, Read, Update, Delete) terhadap daftar kontak yang disimpan dalam memori komputer. Aplikasi ini menggunakan antarmuka berbasis konsol yang berisi menu-menu interaktif untuk menambah, melihat, mengubah, menghapus, mencari, dan mengurutkan kontak.

Code Explanation

Imports and Initialization

	import re
	import uuid

	contacts = []
- "re": Module for regular expressions used in validating inputs.
- "uuid": Module to generate unique IDs (UUID) for each contact.
- "contacts": A list to store contact dictionaries.

Main Menu Function:
def main_menu():
    print("\nYellow Pages - Main Menu")
    print("1. Read Menu")
    print("2. Create Menu")
    print("3. Update Menu")
    print("4. Delete Menu")
    print("5. Search Menu")
    print("6. Sort Menu")
    print("7. Detail Menu")
    print("8. Exit")

- Fungsi ini menampilkan opsi menu utama kepada pengguna.  
- Displays the main menu options for interacting with contacts.

Read Menu Function:
	
	def read_menu():
    	if not contacts:
        	print("No contacts found.")
    	else:
        	print("{:<10} {:<20} {:<15}".format("ID", "Name", "Phone"))
        	for contact in contacts:
            	print("{:<10} {:<20} {:<15}".format(contact['id'], contact['name'], format_phone(contact['phone'])))
- Fungsi ini menampilkan semua kontak jika ada, atau pesan jika daftar kontak kosong.
- Lists all contacts if any exist, displaying their ID, name, and formatted phone number.

Validation and Formatting Functions:
def is_valid_name(name):
    return bool(re.match(r'^[A-Za-z\s]+$', name))

def is_valid_phone(phone):
    phone = phone.lstrip('0')
    return bool(re.match(r'^\+?62\d{9,11}$', phone))

def format_phone(phone):
    phone = phone.lstrip('0')
    if not phone.startswith('+62'):
        phone = '+62' + phone
    return phone

- is_valid_name(name): Validates that the name consists only of alphabetic characters and spaces.
- is_valid_phone(phone): Validates the phone number format, ensuring it starts with optional +62 and has 9 to 11 digits.
- format_phone(phone): Formats the phone number by removing leading zeroes and ensuring it starts with +62.
- 

Function Check for Unique Contacts:

	def is_unique_contact(name, phone):
    	for contact in contacts:
        	if contact['name'].lower() == name.lower() and contact['phone'] == phone:
            	return False
    	return True
- Fungsi ini memeriksa apakah kontak dengan nama dan nomor telepon yang sama sudah ada.
- Checks if a contact with the same name and phone number already exists in the contacts list.

Create Menu Function:
def create_menu():
    while True:
        name = input("Enter name: ")
        if not is_valid_name(name):
            print("Invalid name. Please enter only alphabetic characters and spaces.")
            continue
        break

    while True:
        phone = input("Enter phone number (9-11 digits): ")
        if not is_valid_phone(phone):
            print("Invalid phone number format. Please enter 9 to 11 digits.")
            continue
        break

    if is_unique_contact(name, phone):
        contact_id = str(uuid.uuid4())[:8]
        contacts.append({'id': contact_id, 'name': name, 'phone': format_phone(phone)})
        print("Contact added successfully.")
    else:
        print("Contact with this name and phone number already exists.")
- Fungsi ini memungkinkan pengguna menambahkan kontak baru setelah memvalidasi nama dan nomor telepon.

Update Menu Function:
def update_menu():
    read_menu()
    try:
        contact_id = input("Enter the ID of the contact to update: ")
        contact = next((c for c in contacts if c['id'] == contact_id), None)
        if contact:
            while True:
                name = input("Enter new name: ")
                if not is_valid_name(name):
                    print("Invalid name. Please enter only alphabetic characters and spaces.")
                    continue
                break

            while True:
                phone = input("Enter new phone number (9-11 digits): ")
                if not is_valid_phone(phone):
                    print("Invalid phone number format. Please enter 9 to 11 digits.")
                    continue
                break

            if is_unique_contact(name, phone) or (contact['name'].lower() == name.lower() and contact['phone'] == phone):
                contact['name'] = name
                contact['phone'] = format_phone(phone)
                print("Contact updated successfully.")
            else:
                print("Contact with this name and phone number already exists.")
        else:
            print("Invalid contact ID.")
    except ValueError:
        print("Invalid input. Please enter a valid ID.")
- Fungsi ini memperbarui informasi kontak yang ada setelah memvalidasi detail baru.
	
Delete Menu Function:
def delete_menu():
    read_menu()
    try:
        contact_id = input("Enter the ID of the contact to delete: ")
        contact = next((c for c in contacts if c['id'] == contact_id), None)
        if contact:
            contacts.remove(contact)
            print("Contact deleted successfully.")
        else:
            print("Invalid contact ID.")
    except ValueError:
        print("Invalid input. Please enter a valid ID.")
- Fungsi ini menghapus kontak berdasarkan ID yang diberikan.

Search Menu Function:
def search_menu():
    search_term = input("Enter name or phone number to search: ")
    results = [contact for contact in contacts if search_term.lower() in contact['name'].lower() or search_term in contact['phone']]
    
    if results:
        print("{:<10} {:<20} {:<15}".format("ID", "Name", "Phone"))
        for contact in results:
            print("{:<10} {:<20} {:<15}".format(contact['id'], contact['name'], format_phone(contact['phone'])))
    else:
        print("No contacts found.")
- Fungsi ini mencari kontak berdasarkan nama atau nomor telepon.

Sort Menu Function:
def sort_menu():
    print("\nSort Contacts")
    print("1. By Name")
    print("2. By Phone Number")
    choice = input("Select an option: ")

    if choice == '1':
        sorted_contacts = sorted(contacts, key=lambda x: x['name'])
        print("\nContacts sorted by name:")
    elif choice == '2':
        sorted_contacts = sorted(contacts, key=lambda x: x['phone'])
        print("\nContacts sorted by phone number:")
    else:
        print("Invalid option. Returning to main menu.")
        return
    
    print("{:<10} {:<20} {:<15}".format("ID", "Name", "Phone"))
    for contact in sorted_contacts:
        print("{:<10} {:<20} {:<15}".format(contact['id'], contact['name'], format_phone(contact['phone'])))
- Fungsi ini mengurutkan kontak berdasarkan nama atau nomor telepon.

 Detail Menu Function:
def detail_menu():
    read_menu()
    try:
        contact_id = input("Enter the ID of the contact to view details: ")
        contact = next((c for c in contacts if c['id'] == contact_id), None)
        if contact:
            print(f"\nID: {contact['id']}")
            print(f"Name: {contact['name']}")
            print(f"Phone: {format_phone(contact['phone'])}")
        else:
            print("Invalid contact ID.")
    except ValueError:
        print("Invalid input. Please enter a valid ID.")
- Fungsi ini menampilkan informasi detail tentang kontak tertentu.

Main Function:

	def main():
    	while True:
        	main_menu()
        	choice = input("Select an option: ")

        	if choice == '1':
            	read_menu()
        	elif choice == '2':
            	create_menu()
        	elif choice == '3':
            	update_menu()
        	elif choice == '4':
            	delete_menu()
        	elif choice == '5':
            	search_menu()
        	elif choice == '6':
            	sort_menu()
        	elif choice == '7':
            	detail_menu()
        	elif choice == '8':
            	print("Exiting Yellow Pages. Goodbye!")
            	break
        	else:
            	print("Invalid option. Please select a number between 1 and 8.")
if __name__ == "__main__":
    main()
- Ini adalah fungsi utama yang menggerakkan program. Fungsi ini menampilkan menu utama dan memanggil fungsi yang sesuai berdasarkan input pengguna.

Catatan Tambahan:
Pastikan file disimpan dengan ekstensi .py sebelum mengunggahnya ke GitHub.
Sertakan file .gitignore untuk mengecualikan file yang tidak perlu dari unggahan.
Pertimbangkan untuk menambahkan file README.md untuk menjelaskan cara menggunakan aplikasi ini.


Conclusion
This application provides basic yet essential functionalities for managing contacts efficiently. It demonstrates fundamental concepts such as input validation, list manipulation, and menu-driven program structure in Python.

This code can be further expanded with additional features like data persistence (using databases or file storage), more robust error handling, or integration with other systems.
Feel free to customize, enhance, or adapt this codebase based on specific requirements or further learning objectives.

  
