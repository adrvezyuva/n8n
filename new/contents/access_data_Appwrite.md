## Accessing Data in Appwrite  
### Python Guide
### REST API 


### First, you have to import the required modules:

If you get an import warning message, you can add ` # type: ignore  ` to suppress it.

```python
from appwrite.client import Client # type: ignore
from appwrite.services.users import Users # type: ignore
from appwrite.id import ID # type: ignore
from appwrite.services.databases import Databases # type: ignore
```

### Initialize the Appwrite client and set the endpoint, project ID and API key for it 

```python
client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)
```

### Initialize the Users service

```python
users = Users(client)
```

* ### Create User
```python
# Generates a user with an unique ID 
# You have to set email, phone number, password, and name
# It is not required to enter data for all of them

result = users.create(ID.unique(), 
                      email = "email@example.com", 
                      phone = "+1234567890", 
                      password = "password", 
                      name = "Max Mustermann")
```

* ### Count Nr. of Users
```python
# Fetch the list of users
response = users.list()

# Count the number of users
user_count = response['total']

# Print the number of users
print(f"Total number of users: {user_count}")
```
```json
Total number of users: 7
```

* ### Delete User by Name
#### _Important Note:_
This will delete only one user with exactly the same name, that you have entered. If there are more existing users with this name, it is likely to be deleted the first created one.

```python
# This function deletes a user from the Appwrite database if the user's name matches the given name.

def delete_user_by_name(name): 
    
    # Fetch a list of all users in the Appwrite database
    users_list = users.list() 
    
    # Iterate over each user in the list
    for user in users_list['users']: 

        # Check if the user's name matches the given name
        if user['name'] == name: 

            # Delete the user from the Appwrite database
            users.delete(user['$id']) 

            # Print a message to confirm that the user was deleted
            print(f"User with name {name} deleted.") 

            # Return from the function
            return

    # If no user with the given name was found, print a message
    print(f"No user found with name {name}.")

# Call the function
delete_user_by_name ('Max Mustermann') # Replace 'Max Mustermann' with exactly the same name of the user you want to delete
```
__Output :__
```json
User with name Max Mustermann deleted.
----------------------------------------
No user found with name Max Mustermann.
```

* ### Delete Users by Name

```python
# This function deletes all users from the Appwrite database whose name contains the string "max"

def delete_users_by_name():
   
    users_list = users.list()

    for user in users_list['users']:

        # Check if the user's name contains the string "max"
        # Replace "max" with the name of the user you want to delete
        if "max" in user['name'].lower(): # Convert string to lowercase
            users.delete(user['$id'])
            print(f"User with name {user['name']} deleted.")
    print("All users have been deleted.")

delete_users_by_name()
```
If there are users with the names __Max Mustermann__, __maX mustermann__ and __mAx MUSTERMANN__ in the Appwrite, this would be the __output :__
```json
User with name Max Mustermann deleted.
User with name maX mustermann deleted.
User with name mAx MUSTERMANN deleted.
All users have been deleted.
----------------------------------------
All users have been deleted.
```

* ### Delete User by ID
```python
def delete_user_by_id(ID):
   
    users_list = users.list()
    
    for user in users_list['users']:
        # If the user's ID matches the given ID, delete the user and print a success message
        if user['$id'] == ID:
            users.delete(user['$id'])
            print(f"User with ID {ID} deleted.")
            return
    print(f"No user found with ID {ID}.")

# Replace '123456789' with the ID of the user you want to delete
delete_user_by_id ('123456789') 
```
__Output :__
```json
User with ID 123456789 deleted.
----------------------------------------
No user found with ID 123456789.
```

* ### Delete User by Email
```python
def delete_user_by_email(email):
    
    users_list = users.list()

    for user in users_list['users']:
        # If the user's email matches the given email, delete the user and print a success message
        if user['email'] == email:
            users.delete(user['$id'])
            print(f"User with email {email} deleted.")
            return
    print(f"No user found with email {email}.")

# Replace 'email@example.com' with the email address of the user you want to delete
delete_user_by_email('email@example.com')
```
__Output :__

```json
User with email email@example.com deleted.
----------------------------------------
No user found with email email@example.com.
```

* ### Delete User with No Last Activity
```python
def delete_all_users_with_no_last_activity():

    users_list = users.list()

    for user in users_list['users']:
        if 'lastSignIn' not in user: # Checks if the 'lastSignIn' field is not present in the user object 
            users.delete(user['$id'])
            print(f"User with ID {user['$id']} deleted.")
    print("All users with no last activity have been deleted.")

delete_all_users_with_no_last_activity()
```
__Output :__
```json
User with ID 664d9c410017525e0ae1 deleted.
All users with no last activity have been deleted.
----------------------------------------
All users with no last activity have been deleted.
```

* ### Delete User with Status False (Blocked)
```python
def delete_user_if_status_is_false():
    users_list = users.list()
    for user in users_list['users']:
        if not user['status']:
            users.delete(user['$id'])
            print(f"User {user['name']} - ID {user['$id']} - deleted.")
    print("All blocked users have been deleted.")

delete_user_if_status_is_false()
```
__Output :__
```json
User Max Mustermann - ID 664dd9e8d3c349097cc3 - deleted.
User Michaela Test - ID 664dda1fa4ae0375b296 - deleted.
All blocked users have been deleted.
----------------------------------------
All blocked users have been deleted.
```

* ### Check User's Status with ID
 
```python
def check_user_status(user_id):
    user = users.get(user_id)
    print(f"User with ID {user['$id']} has status {user['status']}.")

# Replace "12345" with the user's ID
check_user_status("12345")
```
__Output :__
```json
User with ID 12345 has status True.
```

* ### List All Users
```python
result = users.list()

# Iterating over the list of users and printing available data
for user in result['users']:
    print(f"ID: {user['$id']}, Name: {user['name']}, Email: {user['email']}, Phone: {user['phone']}, Status: {user['status']}")
```
__Output :__
```json
ID: 12345, Name: Max Mustermann, Email: example@mail.com, Phone: +788452145, Status: True
ID: 987654321, Name: Lutz Beispiel, Email: lutz@example.com, Phone: +874512, Status: False
ID: 657843902, Name: Michaela Test, Email: micha@example.com, Phone: +789451327, Status: True
```

* ### List All Users & check their Statuses
```python
def list_users_and_check_status():
    users_list = users.list()
    for user in users_list['users']:
        print(f"{user['name']} - ID {user['$id']} - has status {user['status']}.")

list_users_and_check_status()
```
__Output :__
```json
Max Mustermann - ID 12345 - has status True.
Michaela Test - ID 664ddb7d1f8e96a49f50 - has status True.
Lutz Beispiel - ID 987654321 - has status False.
```

* ### Edit User's Data
```python
user_id = "654321"

# Calling the get() method on the Users instance to retrieve the user by their ID
old_user = users.get(user_id)

# Extracting the old user's data from the user object
old_name = old_user['name']
old_email = old_user['email']
old_phone = old_user['phone'] 

# Print the old user's data
print(f"Old Data - Name: {old_name}, ID: {old_user['$id']}, Email: {old_email}, Phone: {old_phone}, Status: {old_user['status']}")

# Defining variables for the new user's data
new_name = 'Gabriella Test'
new_email = 'test@example.com'
new_phone = '+667788'

# Calling the update_x() method on the Users instance to update the user's data by using the ID
result_name = users.update_name(user_id, new_name)
result_email = users.update_email(user_id, new_email)
result_phone = users.update_phone(user_id, new_phone)

# Print the new user's data
print(f"New Data - Name: {result_name['name']}, ID: {result_email['$id']}, Email: {result_email['email']}, Phone: {result_phone['phone']}, Status: {result_email['status']}")
```
__Output :__
```json
Old Data - Name: Mario Example, ID: 654321, Email: mario@example.com, Phone: +112233, Status: True
New Data - Name: Gabriella Test, ID: 654321, Email: test@example.com, Phone: +667788, Status: True
```

* ### Create Database 
```python
# Creating a new instance of the Databases class using the client instance
databases = Databases(client)

# Replace '12345', 'My Database' with an ID and a name for the new database
result = databases.create('12345', 'My Database')
```

* ### Delete Database 
```python
databases = Databases(client)

# Attempt to delete the database with the provided ID
def delete_database(database_id):
    
# The try block for testing the code for errors
# The except block for handling the error
# If an error occurs in the try block, the code in the except block is executed
    try:
        databases.delete(database_id)
        print(f"Database with ID {database_id} has been deleted successfully.")
    except Exception as e:
        # If an error occurs, catch the exception and print an error message
        print(f"An error occurred: {e}.")

# Replace '12345' with an ID
database_id = '12345'
delete_database(database_id)
```
__Output :__
```json
Database with ID 12345 has been deleted successfully.
----------------------------------------
An error occurred: Database not found.
```

* ### List Databases

#### Listing and counting
```python
from appwrite.client import Client # type: ignore
from appwrite.services.storage import Storage # type: ignore
from appwrite.services.databases import Databases # type: ignore

client = Client()

(client
  .set_endpoint('Endpoint') 
  .set_project('Project ID') 
  .set_key('API key')
)

storage = Storage(client)

databases = Databases(client)
print(f"Total databases: {databases.list()['total']}")
for db in databases.list()['databases']:
    print(db['$id'] + " : " + db['name'])
```
__Output :__
```json
Total databases: 3
6644c0f71135b003e99f : VirtualEnvironment
664f0863ae0f6420b1b2 : My World
664f086f68bea8650eab : Test Database
```

#### Try & Except
```python
databases = Databases(client)

def list_databases():
    try:
        response = databases.list()
        for database in response['databases']:
            print(f"ID: {database['$id']}, Name: {database['name']}")
    except Exception as e:
        print(f"An error occurred: {e}")

list_databases()
```
__Output :__
```json
ID: 6644c0f71135b003e99f, Name: VirtualEnvironment
ID: 664f0863ae0f6420b1b2, Name: My World
ID: 664f086f68bea8650eab, Name: database 2
```

* ### Update Database's Name
```python
databases = Databases(client)

# Replace "664f086f68bea8650eab" with your database ID
database_id = "664f086f68bea8650eab"

# Fetch the old database details
old_database = databases.get(database_id)
old_name = old_database['name']

print(f"Old Data - Name: {old_name}, ID: {old_database['$id']}")

# Update the database name
new_name = 'Test Database'

# Updating the database requires the database ID and the name as a parameter directly, not in a dictionary
result = databases.update(database_id=database_id, name=new_name)

updated_name = result['name']

print(f"New Data - Name: {updated_name}, ID: {result['$id']}")
```
__Output :__
```json
Old Data - Name: database 2, ID: 664f086f68bea8650eab
New Data - Name: Test Database, ID: 664f086f68bea8650eab
```












