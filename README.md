# JCDSOL15-CAPSTONE1-RMGT
Yellow Pages - Contact Management Application
Overview
This Python application, named Yellow Pages, serves as a digital phone book where users can manage contacts through various operations such as adding, updating, deleting, searching, sorting, and viewing detailed information about contacts. The application ensures each contact has a unique identifier and validates inputs for names and phone numbers according to specified criteria.

Code Explanation

Imports and Initialization

	import re
	import uuid

	contacts = []
- "re": Module for regular expressions used in validating inputs.
- "uuid": Module to generate unique IDs (UUID) for each contact.
- "contacts": A list to store contact dictionaries.

Main Menu

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
  
- Displays the main menu options for interacting with contacts.

Read Menu
	
	def read_menu():
    	if not contacts:
        	print("No contacts found.")
    	else:
        	print("{:<10} {:<20} {:<15}".format("ID", "Name", "Phone"))
        	for contact in contacts:
            	print("{:<10} {:<20} {:<15}".format(contact['id'], contact['name'], format_phone(contact['phone'])))

- Lists all contacts if any exist, displaying their ID, name, and formatted phone number.

Validation Functions

- is_valid_name(name): Validates that the name consists only of alphabetic characters and spaces.
- is_valid_phone(phone): Validates the phone number format, ensuring it starts with optional +62 and has 9 to 11 digits.
- format_phone(phone): Formats the phone number by removing leading zeroes and ensuring it starts with +62.

Function for Unique Contacts

	def is_unique_contact(name, phone):
    	for contact in contacts:
        	if contact['name'].lower() == name.lower() and contact['phone'] == phone:
            	return False
    	return True
- Checks if a contact with the same name and phone number already exists in the contacts list.

Menu Functions

- Create Menu (create_menu()): Allows users to input a new contact's name and phone number, validates them, generates a unique ID, and adds the contact to contacts.
- Update Menu (update_menu()): Updates an existing contact's name or phone number based on its ID.
- Delete Menu (delete_menu()): Deletes a contact from contacts based on its ID.
- Search Menu (search_menu()): Searches for contacts by name or phone number and displays matching results.
- Sort Menu (sort_menu()): Sorts contacts alphabetically by name or by phone number.
- Detail Menu (detail_menu()): Displays detailed information about a contact based on its ID.

Main Function

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

- Executes the main menu loop, where users can choose different operations until they choose to exit.
Usage
Ensure Python is installed on your system.
Run the yellow_pages.py file in your preferred Python environment.
Follow the prompts to manage contacts using the specified menu options.
Conclusion
This application provides basic yet essential functionalities for managing contacts efficiently. It demonstrates fundamental concepts such as input validation, list manipulation, and menu-driven program structure in Python.

This code can be further expanded with additional features like data persistence (using databases or file storage), more robust error handling, or integration with other systems.

Feel free to customize, enhance, or adapt this codebase based on specific requirements or further learning objectives.

  
