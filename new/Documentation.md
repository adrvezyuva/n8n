# Accessing Data in Appwrite - Visual Studio Code

### First, you have to import the required modules

If you get an import warning message, you can add `# type: ignore` to suppress it.

```python
from appwrite.client import Client # type: ignore
from appwrite.services.users import Users # type: ignore
from appwrite.id import ID # type: ignore
from appwrite.services.databases import Databases # type: ignore
```

### Initialize the Appwrite Client and Set the Endpoint, Project ID and API Key for it

```python
client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)
```

### Initialize the Users Service

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

__Output :__

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

* ### List All Users & Check their Status

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

* ### Try & Except

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

# Appwrite - Functions

#### Sources

__<https://appwrite.io/docs/products/functions/develop>__ <br>
<https://github.com/appwrite/templates> <br>
<https://appwrite.io/docs/references/cloud/server-python/functions> <br>
<https://appwrite.io/docs/products/functions/functions>

## Manual Guide

### Creating a Database - Collection

* Create database

![No Image to show](assets/1.png)

* Note: Database Name = ID

![No Image to show](assets/2.png)

* Create collection, where Collection Name = ID

![No Image to show](assets/3.png)

* Once a collection is created, you can add documents. Here, you can add different attributes.

![No Image to show](assets/4.png)

* Don't forget to check the Setting and the Permissions.

![No Image to show](assets/5.png)

### Collections - Documents - Settings

![No Image to show](assets/6.png)
![No Image to show](assets/7.png)

### Storage - Buckets

* Create bucket, where Bucket Name = ID. Don't forget to check the Settings.
* You are not able to manually delete all files together in the storage bucket. To do so, you can delete the bucket and create a new one with the same details.

![No Image to show](assets/8.png)
![No Image to show](assets/9.png)
![No Image to show](assets/10.png)

 A file in the storage bucket can be downloaded via a download link. Add permissions.

![No Image to show](assets/11.png)

### Create Functions

* Create function

![No Image to show](assets/12.png)

* Enter a Name & select a runtime.
* ID: Parameter must contain at most 36 chars. Valid chars are a-z, A-Z, 0-9, period, hyphen, and underscore. Can't start with a special char

![No Image to show](assets/13.png)

* To be a valid function, you have to upload a tr.gzip file. In the tar.gzip file you need a folder `src` (inside it only the `main.py`) and `requirements.txt` (the text in my case: appwrite <br>
python-dotenv <br>
appwrite <br>
pytz)
* Entrypoint: `src/main.py`
* Add the build settings-commands: `pip install --upgrade pip <br>
pip install -r requirements.txt`

![No Image to show](assets/14.png)

* Add permissions.

![No Image to show](assets/15.png)

* Once the function is created, check the Settings before making a Deployment.
* After every single edit, you have to click `Update` to save the made changes.
* When you have finished, you have to `Redeploy` the function and after that execute it.

![No Image to show](assets/16.png)

* You are able to edit and update the information.

![No Image to show](assets/17.png)

* Check again the entrypoint and the build settings.

![No Image to show](assets/18.png)

* Add events, access and etc.

![No Image to show](assets/19.png)

* Here, you can import your .env file or directly add the env. variables.

![No Image to show](assets/20.png)

## Creating Functions Using the CLI (Command Line Interface)

## 1. CLI Initialization

### 1.1. npm and Node.js

<https://docs.npmjs.com/downloading-and-installing-node-js-and-npm>

* Checking your version of __npm__ and __Node.js__

```json
node -v
npm -v
```

If __npm__ and __Node.js__ are not installed, follow the instructions in the website.

### 1.2. Git Installation

* Check if Git is installed. <br>
If not → <https://git-scm.com/book/en/v2/Getting-Started-Installing-Git>

### 1.3. CLI Installation with npm

* If you have npm set up, run the command below in the PowerShell to install the CLI.

```json
npm install -g appwrite-cli
```

If you get the error:

```json
appwrite : File C:\Users\Roaming\npm\appwrite.ps1 cannot be loaded. The file ... is not digitally signed. You cannot run this script on the current system...
```

* First, open _PowerShell_ with _Run as Administrator_ and then, run this command:

```json
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
```

* After that type ` Y `and press _Enter_.

### 1.4. Verify Installation

After the installation is complete, you can verify the Appwrite CLI is available by checking its version number. (PowerShell)

```json
appwrite -v
```

### 1.5. Appwrite Login

* PowerShell:

```json
appwrite login
```

* Follow the instructions in the Shell, for example:

```json
? Enter your email <your email>
```

```json
? Enter your password <your password>
```

```json
? Enter the endpoint of your Appwrite server <API Endpoint>
```

__Output:__

```json
Success Signed in as user with ID: 1234567890
```

### 1.6. Run your first command

```json
appwrite projects get --projectId 999888777
```

__Output:__

```json
$id : 999888777
$createdAt : 2024-05-15T12:52:06.292+00:00
$updatedAt : 2024-05-15T14:15:07.542+00:00
name : MyFirstProject
...
serviceStatusForGraphql : true
✓ Success
```

### 1.7. Self-signed Certificates

By default, requests to domains with self-signed SSL certificates (or no certificates) are disabled. If you trust the domain, you can bypass the certificate validation using:

```json
appwrite client --selfSigned true
```

### Configurations (Optional)

At any point, if you would like to __change your server's endpoint__, project ID, or self-signed certificate acceptance, use the `client`command:

```json
appwrite client --endpoint https://cloud.appwrite.io/v1
appwrite client --key 23f24gwrhSDgefaY
appwrite client --selfSigned true
appwrite client --reset // Resets your CLI configuration
appwrite client --debug // Prints your current configuration
```

* If you __get stuck__ anywhere, you can always use the `help` command to get the usage examples.

```json
appwrite help
```

### Commands

Commands follows the following general syntax:

```json
appwrite [COMMAND] [OPTIONS]
```

* __Verbose__ </br>
In case of errors with any command, you can get more information about what went wrong using the --verbose flag

```json
appwrite users list --verbose
```

* __JSON__ </br>
By default, output is rendered in a tabular format. To format the output as JSON, use the --json flag.

```json
appwrite users list --json
```

* __Create a User__ </br>
To create a new user in your project, you can use the create command.

```json
appwrite users create --userId "unique()" \
    --email hello@appwrite.io \
    --password very_strong_password
    ```
* __List Users__ </br>
```json
appwrite users list
```

* __List Users__ </br>

```json
appwrite users list
```

* __List Collections__ </br>

```json
appwrite databases listCollections --databaseId [DATABASE_ID]
```

If you wish to parse the output from the CLI, you can request the CLI output in JSON format using the --json flag:

```json
appwrite databases listCollections --databaseId [DATABASE_ID]--json
```

* __Get Collections__ </br>
To get more information on a particular collection, you can make use of the getCollection command and pass in the collectionId.

```json
appwrite databases getCollection --databaseId [DATABASE_ID] --collectionId [COLLECTION_ID]
```

* __Create a Document__ </br>
To create a new document in an existing collection, use the `createDocument` command.

```json
appwrite databases createDocument \
    --databaseId [DATABASE_ID] --collectionId [COLLECTION_ID] \
    --documentId 'unique()' --data '{ "Name": "Iron Man" }' \
    --permissions 'read("any")' 'write("team:abc")' 
```

## 2. Creating a Starter Function

```json
appwrite init function
```

```json
? What would you like to name your function? <give a name>
```

```json
? What ID would you like to have for your function? <type an uniqueID>
 ```

 ```json
 ? What runtime would you like to use? <select by using the arrow keys>
```

__Output:__

```json
? What would you like to name your function? Test
? What ID would you like to have for your function? unique()
? What runtime would you like to use? Python (python-3.9)
✓ Success
```

* The init command will initialize a folder with a starter function code. To deploy the generated code, add dependencies, and deploy the function using the following command:

```json
appwrite deploy function
```

```json
? Which functions would you like to deploy? 
 (*) Test (6655c0d8ea3234f8e15c)
 ```

__Note:__ Press _Space_ to select ... and _Enter_ to  proceed.

```json
ℹ Info Deploying function Test ( 6655c0d8ea3234f8e15c )
ℹ Info Ignoring files using configuration from appwrite.json
✓ Success Deployed Test ( 6655c0d8ea3234f8e15c )
✓ Success Deployed 1 functions
```

## 2. Get Started with Functions

* You have already created and deployed the function ___Test___. <br>
* Now, go to the folder where your function is located and from the ___src___ folder open the file ___main.py___. <br>
On my PC: `C:\Users\12354697\functions\Test\src`

```python
#This is the main.py file

from appwrite.client import Client # type: ignore
import os

# This is your Appwrite function
# It's executed each time we get a request
def main(context):
    # Why not try the Appwrite SDK?
    # client = (
    #     Client()
    #     .set_endpoint("https://cloud.appwrite.io/v1")
    #     .set_project(os.environ["APPWRITE_FUNCTION_PROJECT_ID"])
    #     .set_key(os.environ["APPWRITE_API_KEY"])
    # )

    # You can log messages to the console
    context.log("Hello, Logs!")

    # If something goes wrong, log an error
    context.error("Hello, Errors!")

    # The `ctx.req` object contains the request data
    if context.req.method == "GET":
        # Send a response with the res object helpers
        # `ctx.res.send()` dispatches a string back to the client
        return context.res.send("Hello, World!")

    # `ctx.res.json()` is a handy helper for sending JSON
    return context.res.json(
        {
            "motto": "Build like a team of hundreds_",
            "learn": "https://appwrite.io/docs",
            "connect": "https://appwrite.io/discord",
            "getInspired": "https://builtwith.appwrite.io",
        }
    )
```

Meanwhile, in the _Appwrite_ you can see the function and the deployment.

![No Image to show](assets/Untitled.png)

* __Execute function manually:__ <br>
Click on _Execute now_, <br>
Select a _Path_ and a _Method_ and confirm by clicking on _Execute_. <br>
Now, you can see all made executions in the _Executions_ section.

## 3. Deployment VS Execution

#### Deployment

* Refers to the process of uploading and registering your function's code with Appwrite.
* __Packaging:__ You need to package your function's code and any dependencies into a deployable format (e.g., ZIP file).
* __Uploading:__ You upload this package to Appwrite using the `create_deployment` API.
* __Activation:__ You can choose to activate the deployment immediately or later. Activation makes the deployment the current version of the function that will be executed when the function is invoked.
* __Versioning:__ Each deployment can be versioned, allowing you to manage updates and rollbacks of your functions.

#### Creating and Activating a Deployment

* It is used for creating a new deployment for a function.
* The deployment is created with the code provided as a compressed file and is immediately activated.
* Used when you need to deploy new code for a function. This is typically the step where you upload your function's source code.

#### Creating a Build for an Existing Deployment Deployment

* It requires an existing deployment ID and build ID.
* Used when you need to create a build for an already existing deployment. This might be part of a continuous integration/continuous deployment (CI/CD) pipeline where builds are created and managed separately.

#### Execution

* Refers to running the function that has been deployed.
* When you execute a function, Appwrite takes the code that was uploaded and activated during deployment and runs it.
* Execution can be triggered manually through the Appwrite console, via API calls, or automatically in response to specific events or schedules.
* __Passing Arguments:__ You can pass data to the function execution through input parameters.
* __Retrieving Output:__ The result of the function execution can be retrieved, which includes the function’s output and execution details.
* __Monitoring and Logging:__ Execution logs and status can be monitored for debugging and auditing purposes.

__Summary:__ <br>
__Deployment:__ The process of uploading and registering function code with Appwrite. This involves creating a deployment package and activating it. Each deployment can be versioned.<br>
__Execution:__ The process of running the deployed function. Execution can be triggered manually or programmatically and can include input data. Logs and execution details are available for monitoring.

## 4. Main.py

* ### Create a Function

```python
from appwrite.client import Client # type: ignore
from appwrite.services.functions import Functions # type: ignore

client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)
functions = Functions(client)

result = functions.create(
    function_id = '12345678dcc90aabbccddee', # Replace the ID
    name = 'Test Funct', # Feel free to replace the values
    runtime = 'python-3.9'
)
print (result)
```

__Output :__

```json
{'$id': '12345678dcc90aabbccddee', '$createdAt': '2024-05-28T12:51:35.820+00:00', '$updatedAt': '2024-05-28T12:51:35.826+00:00', 'execute': [], 'name': 'Test Funct', 'enabled': True, 'live': True, 'logging': True, 'runtime': 'python-3.9', 'deployment': '', 'vars': [], 'events': [], 'schedule': '', 'timeout': 15, 'entrypoint': '', 'commands': '', 'version': 'v3', 'installationId': '', 'providerRepositoryId': '', 'providerBranch': '', 'providerRootDirectory': '', 'providerSilentMode': False}
```

* ### Update Function

```python
from appwrite.client import Client # type: ignore
from appwrite.services.users import Users # type: ignore
from appwrite.services.functions import Functions # type: ignore

client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)
functions = Functions(client)

result = functions.update(
    function_id ='12345678dcc90aabbccddee', # Replace the ID
    name = 'My World', # Replace the values
    runtime = 'python-3.9', # optional
    timeout = 2, # optional
    enabled = True, # optional
    execute = ["any"], # optional
    events = [], # optional
    schedule = '', # optional
    logging = False, # optional
    entrypoint = '<ENTRYPOINT>', # optional
    commands = '<COMMANDS>', # optional
    installation_id = '<INSTALLATION_ID>', # optional
    provider_repository_id = '<PROVIDER_REPOSITORY_ID>', # optional
    provider_branch = '<PROVIDER_BRANCH>', # optional
    provider_silent_mode = False, # optional
    provider_root_directory = '<PROVIDER_ROOT_DIRECTORY>' # optional
)

)
print (result)
```

__Output :__

```json
{'$id': '12345678dcc90aabbccddee', '$createdAt': '2024-05-28T11:32:40.959+00:00', '$updatedAt': '2024-05-28T13:27:15.669+00:00', 'execute': [], 'name': 'My World', 'enabled': True, 'live': False, 'logging': True, 'runtime': 'python-3.9', 'deployment': '12345678dcc90aabbccddee3', 'vars': [], 'events': [], 'schedule': '', 'timeout': 2, 'entrypoint': 'src/main.py', 'commands': '', 'version': 'v3', 'installationId': '', 'providerRepositoryId': '', 'providerBranch': '', 'providerRootDirectory': '', 'providerSilentMode': False}
```

* ### Delete Function

```python
from appwrite.client import Client # type: ignore
from appwrite.services.functions import Functions # type: ignore

client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)

functions = Functions(client)

result = functions.delete(
    function_id = '12345678dcc90aabbccddee' # Replace the ID
)
print ("Function deleted.")

```

__Output :__

```json
Function deleted.
```

* ### Update Function Deployment

```python
from appwrite.client import Client # type: ignore
from appwrite.services.functions import Functions # type: ignore

client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)

functions = Functions(client)

result = functions.update_deployment(
    function_id = '12345678dcc90aabbccddee', # Replace the ID
    deployment_id = '66558c621bd2885984a0', # Replace the ID
)
print (result)
print ("Updated!")
```

__Output :__

```json
'$id': '12345678dcc90aabbccddee', '$createdAt': '2024-05-28T11:32:40.959+00:00', '$updatedAt': '2024-05-28T13:42:23.321+00:00', 'execute': [], 'name': 'My World', 'enabled': True, 'live': True, 'logging': True, 'runtime': 'python-3.9', 'deployment': '12345678dcc90aabbccddee', 'vars': [], 'events': [], 'schedule': '', 'timeout': 2, 'entrypoint': 'src/main.py', 'commands': '', 'version': 'v3', 'installationId': '', 'providerRepositoryId': '', 'providerBranch': '', 'providerRootDirectory': '', 'providerSilentMode': False}
Updated!
```

* ### Get Function Request

```python
from appwrite.client import Client # type: ignore
from appwrite.services.functions import Functions # type: ignore

client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)

functions = Functions(client)

result = functions.get(
    function_id = '12345678dcc90aabbccddee' # Replace the ID
)
print("Function:")
print(result)

```

__Output :__

```json
Function:
{'$id': '12345678dcc90aabbccddee', '$createdAt': '2024-05-28T11:32:40.959+00:00', '$updatedAt': '2024-05-28T13:42:23.321+00:00', 'execute': [], 'name': 'My World', 'enabled': True, 'live': True, 'logging': True, 'runtime': 'python-3.9', 'deployment': '12345678dcc90aabbccddee', 'vars': [], 'events': [], 'schedule': '', 'timeout': 2, 'entrypoint': 'src/main.py', 'commands': '', 'version': 'v3', 'installationId': '', 'providerRepositoryId': '', 'providerBranch': '', 'providerRootDirectory': '', 'providerSilentMode': False}
```

* ### List Functions

```python
from appwrite.client import Client # type: ignore
from appwrite.services.functions import Functions # type: ignore

client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)

functions = Functions(client)

result = functions.list(
    queries = [], # optional
    search = '<SEARCH>' # optional
)
print ("Existing Functions:")
print (result)
```

__Output :__

```json
Existing Functions:
{'total': 8, 'functions': [{'$id': '12345678dcc90aabbccddee', ...
...
...
```

* ### List Runtimes Using the Appwrite Functions Service

```python
from appwrite.client import Client # type: ignore
from appwrite.services.functions import Functions # type: ignore

client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)

functions = Functions(client) # type: ignore

print("Client and Functions service initialized.")

result = functions.list_runtimes()
print("Runtimes listed.")
print (result)
```

__Output :__

```json
Client and Functions service initialized.
Runtimes listed.
{'total': 6, 'runtimes': [{'$id': 'node-16.0', 'name': 'Node.js', 'version': '16.0', 'base': 'node:16.0-alpine3.13', 'image': 'openruntimes/node:v3-16.0', 'logo': 'node.png', 'supports': ['x86', 'arm64', 'armv7', 'armv8']}, ........... {'$id': 'dotnet-7.0', 'name': '.NET', 'version': '7.0', 'base': 'mcr.microsoft.com/dotnet/sdk:7.0-alpine3.18', 'image': 'openruntimes/dotnet:v3-7.0', 'logo': 'dotnet.png', 'supports': ['x86', 'arm64', 'armv7', 'armv8']}]}
```

* ### Create Deployment

```python
from appwrite.client import Client # type: ignore
from appwrite.services.functions import Functions # type: ignore

client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)

functions = Functions(client)

result = functions.create_deployment(
    function_id = '12345678dcc90aabbccddee', # Replace the ID 
    code = InputFile.from_path('C:/Users/.../tar.gz'), # Replace the path
    activate = True
)
print (result)
print ("Deployment created!")

```

__Output :__

```json
{'$id': '12345678dcc90aabbccddee', '$createdAt': '2024-05-28T14:06:18.894+00:00', '$updatedAt': '2024-05-28T14:06:18.894+00:00', 'type': 'manual', 'resourceId': '12345678dcc90aabbccddeec', 'resourceType': 'functions', 'entrypoint': 'src/main.py', 'size': 0, 'buildId': '', 'activate': True, 'status': '', 'buildLogs': '', 'buildTime': 0, 'providerRepositoryName': '', 'providerRepositoryOwner': '', 'providerRepositoryUrl': '', 'providerBranch': '', 'providerCommitHash': '', 'providerCommitAuthorUrl': '', 'providerCommitAuthor': '', 'providerCommitMessage': '', 'providerCommitUrl': '', 'providerBranchUrl': ''}
Deployment created!
```

* ### Get Deployment

```python
from appwrite.client import Client # type: ignore
from appwrite.services.functions import Functions # type: ignore
# Import the json module for working with JSON data
import json 
# Import the sys module for system-specific parameters and functions
import sys
# Import the io module for input/output operations
import io

# Ensure the correct encoding is used for the output
sys.stdout = io.TextIOWrapper(sys.stdout.buffer, encoding='utf-8')

client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)

functions = Functions(client)

result = functions.get_deployment(
    function_id = '12345678dcc90aabbccddee', # Replace the ID
    deployment_id = '6655e4dad7b1b7305abe', # Replace the ID
)

# Convert the result to a JSON string using the json.dumps function, ensuring ASCII characters are not escaped and indenting with 2 spaces
# Print the resulting JSON string
print(json.dumps(result, ensure_ascii=False, indent=2)) # type: ignore
```

__Output :__

```json
{
  "$id": "6655e4dad7b1b7305abe",
  "$createdAt": "2024-05-28T14:06:18.894+00:00",
  "$updatedAt": "2024-05-28T14:06:18.910+00:00",
...
  "providerBranchUrl": ""
}
```

* ### Delete Deployment

```python
from appwrite.client import Client # type: ignore
from appwrite.services.functions import Functions # type: ignore

client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)

functions = Functions(client)

result = functions.delete_deployment(
    function_id = '12345678dcc90aabbccddee', # Replace the ID
    deployment_id = '6655e4dad7b1b7305abe' # Replace the ID
)
print (result)
print ("Deployment deleted.")
```

__Output :__

```json
b''
Deployment deleted.
```

* ### List Deployments

```python
from appwrite.client import Client # type: ignore
from appwrite.services.functions import Functions # type: ignore
import json
import sys
import io

# Ensure the correct encoding is used for the output
sys.stdout = io.TextIOWrapper(sys.stdout.buffer, encoding='utf-8')

client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)

functions = Functions(client)

result = functions.list_deployments(
    function_id = '12345678dcc90aabbccddee', # Replace the ID
    queries = [], # optional
    search = '<SEARCH>' # optional
)

# Convert the result to a JSON string and print it
print(json.dumps(result, ensure_ascii=False, indent=2)) # type: ignore
```

__Output :__

```json
{
  "total": 3,
  "deployments": [
    {
      "$id": "6655c429e808e0692023",...
      ...
            "providerBranchUrl": ""
    }
  ]
}
```

* ### Download Deployment

```python
from appwrite.client import Client # type: ignore
from appwrite.services.functions import Functions # type: ignore

client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)

functions = Functions(client)

result = functions.download_deployment( # type: ignore
    function_id = '12345678dcc90aabbccddee', # Replace the ID
    deployment_id = '6655df32c9937ff382d0' # Replace the ID
)
print (result)
```

__Output :__

```json
b'\x1f\x8b\x08\.....x00&\x00\x00'
```

* ### Create Execution

```python
from appwrite.client import Client # type: ignore
from appwrite.services.users import Users # type: ignore
from appwrite.id import ID # type: ignore
from appwrite.services.functions import Functions # type: ignore

client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)

users = Users(client)
fun = Functions (client)
response = fun.create_execution("12345678dcc90aabbccddee") # Replace the ID
print (response)
```

__Output :__

```json
{'$id': '12345678dcc90aabbccddeecaf'...
...
'duration': 0.5295460224151611}
```

* ### List Executions

```python
from appwrite.client import Client # type: ignore
from appwrite.services.functions import Functions # type: ignore

client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)

functions = Functions(client)

result = functions.list_executions(
    function_id = '12345678dcc90aabbccddee', # Replace the ID
    queries = [], # optional
    search = '<SEARCH>' # optional
)
print (result)
```

__Output :__

```json
{'total': 17, 'executions': ...
...
'duration': 0.51221704483032}]}
```

* ### Get Execution

```python
from appwrite.client import Client # type: ignore
from appwrite.services.users import Users # type: ignore
from appwrite.id import ID # type: ignore
from appwrite.services.functions import Functions # type: ignore

client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)

users = Users(client)
fun = Functions (client)
response = fun.get_execution("12345678dcc90aabbccddee" , "66558731a6b046a5f67d") # Replace: project ID and execution ID
print (response)
```

__Output :__

```json
{'$id': '6655cccd842db2e1b235', '$createdAt': '2024-05-28T12:23:41.541+00:00', '$updatedAt': '2024-05-28T12:23:42.600+00:00', '$permissions': ['read("user:6644aee2e5dbfad8fada")'], 'functionId': '6655c0d8ea3234f8e15c', 'trigger': 'http', 'status': 'completed', 'requestMethod': 'GET', 'requestPath': '/', 'requestHeaders': [], 'responseStatusCode': 200, 'responseBody': '', 'responseHeaders': [{'name': 'content-length', 'value': '13'}, {'name': 'content-type', 'value': 'text/plain; charset=utf-8'}], 'logs': 'Hello, Logs!', 'errors': 'Hello, Errors!', 'duration': 0.52593493461609}
```

* ### Create Variable

```python
from appwrite.client import Client # type: ignore
from appwrite.id import ID # type: ignore
from appwrite.services.functions import Functions # type: ignore

client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)

functions = Functions(client)

result = functions.create_variable(
    function_id = '12345678dcc90aabbccddee', # Replace the ID
    key = 'var4', # Replace the value
    value = '25' # Replace the value
)
print (result)
```

__Output :__

```json
{'$id': '6655f217dcb8c82cbbbf', '$createdAt': '2024-05-28T15:02:47.904+00:00', '$updatedAt': '2024-05-28T15:02:47.904+00:00', 'key': 'var4', 'value': '25', 'resourceType': 'function', 'resourceId': '6655c0d8ea3234f8e15c'}
```

* ### List Variables

```python
from appwrite.client import Client # type: ignore
from appwrite.id import ID # type: ignore
from appwrite.services.functions import Functions # type: ignore

client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)

functions = Functions(client)

result = functions.list_variables(
    function_id = '12345678dcc90aabbccddee', # Replace the ID
)
print (result)
```

__Output :__

```json
{'total': 1, 'variables': [{'$id': '6655f217dcb8c82cbbbf', '$createdAt': '2024-05-28T15:02:47.904+00:00', '$updatedAt': '2024-05-28T15:02:47.904+00:00', 'key': 'var4', 'value': '25', 'resourceType': 'function', 'resourceId': '6655c0d8ea3234f8e15c'}]}
```

* ### Get Variable

```python
from appwrite.client import Client # type: ignore
from appwrite.services.users import Users # type: ignore
from appwrite.id import ID # type: ignore
from appwrite.services.functions import Functions # type: ignore

client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)

functions = Functions(client)

result = functions.get_variable(
    function_id = '12345678dcc90aabbccddee', # Replace the ID
    variable_id = '6655f217dcb8c82cbbbf' # Replace the ID
)
print (result)
```

__Output :__

```json
{'$id': '6655f217dcb8c82cbbbf', '$createdAt': '2024-05-28T15:02:47.904+00:00', '$updatedAt': '2024-05-28T15:02:47.904+00:00', 'key': 'var4', 'value': '25', 'resourceType': 'function', 'resourceId': '6655c0d8ea3234f8e15c'}
```

* ### Update Variable

```python
from appwrite.client import Client # type: ignore
from appwrite.services.users import Users # type: ignore
from appwrite.id import ID # type: ignore
from appwrite.services.functions import Functions # type: ignore

client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)

functions = Functions(client)

result = functions.update_variable(
    function_id = '12345678dcc90aabbccddee', # Replace the ID
    variable_id = '6655f217dcb8c82cbbbf', # Replace the ID
    key = 'new', # optional
    value = '44' # optional
)
print(result)
```

__Output :__

```json
{'$id': '6655f217dcb8c82cbbbf', '$createdAt': '2024-05-28T15:02:47.904+00:00', '$updatedAt': '2024-05-28T15:02:47.904+00:00', 'key': 'new', 'value': '44', 'resourceType': 'function', 'resourceId': '6655c0d8ea3234f8e15c'}
```

* ### Delete Variable

```python
from appwrite.client import Client # type: ignore
from appwrite.id import ID # type: ignore
from appwrite.services.functions import Functions # type: ignore

client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)

functions = Functions(client)

result = functions.delete_variable(
    function_id = '12345678dcc90aabbccddee', # Replace the ID
    variable_id = '6655f217dcb8c82cbbbf' # Replace the ID
)
print ("Deleted variable.")
```

__Output :__

```json
Deleted variable.
```

* ### Create Build

__Important Note:__ Here, you first need to have the _build_id_ and the _deployment_id_. To get them easier, __list the deployments__ and use the needed build_id to initiate a build process. After running the code, redeploy your function to apply the latest changes.

```python
# Create Build
from appwrite.client import Client # type: ignore
from appwrite.services.functions import Functions # type: ignore

client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)

functions = Functions(client)
result = functions.create_build(
    function_id = '6655c0d8ea3234f8e15c', # Replace the ID
    deployment_id = '6655f6a33dbe5357605f', # Replace the ID
    build_id = '6655f6a33eea3a34acad' # Replace the ID
)

print ("Ready.")
```

__Output :__

```json
Ready.
```

![No Image to show](assets/Build.png)

* ### List Databases

```python
from appwrite.client import Client # type: ignore
from appwrite.services.databases import Databases # type: ignore
from appwrite.services.users import Users # type: ignore
from appwrite.services.functions import Functions # type: ignore

client = Client()

(client
  .set_endpoint('Replace the text with the endpoint.') 
  .set_project('Replace the text with the project ID.') 
  .set_key('Replace the text with the API key.') 
)

functions = Functions(client)

users = Users(client)
fun = Functions(client)
databases = Databases(client)

# Create a function execution
response = fun.create_execution("12345678dcc90aabbccddee") # Replace with a function ID

# List databases
databases_list = databases.list()

print(databases_list)

```

__Output :__

```json
{'total': 3, 'databases': [{'$id': '6644c0f71135b003e99f', 'name': 'VirtualEnvironment', '$createdAt': '2024-05-15T14:04:39.070+00:00', '$updatedAt': '2024-05-15T14:04:39.070+00:00', 'enabled': True}, {'$id': '664f0863ae0f6420b1b2', 'name': 'My World', '$createdAt': '2024-05-23T09:12:03.713+00:00', '$updatedAt': '2024-05-23T09:12:03.713+00:00', 'enabled': True}, {'$id': '664f086f68bea8650eab', 'name': 'Test Database', '$createdAt': '2024-05-23T09:12:15.429+00:00', '$updatedAt': '2024-05-23T11:24:53.938+00:00', 'enabled': True}]}
```

# Creating and Deploying Functions Using the CLI - Summary

* 1. Log in to Appwrite CLI

```sh
appwrite login
```

* 2. Navigate to the project directory

```sh
cd C:\python_files\Git\synergy-rnd-tools\prototyping\appwrite-py\collect-md-function
```

* 3. Prepare the function files

```sh
copy C:\path\to\your\function.py main.py
```

* 4. Create the zip file

```sh
powershell Compress-Archive -Path .\* -DestinationPath .\collect-md-function.zip
```

* 5. Initialize the project (if not already initialized)

```sh
ayppwrite init project
```

* 6. Create the function

```sh
appwrite functions create --functionId "New" --name "New" --runtime python-3.9 --entrypoint="main.py" --execute="any"
```

* 7. Create the deployment

```sh
appwrite functions createDeployment ^ --functionId=Hllo ^ --entrypoint='main.py' ^ --code="HiWorld.zip" ^ --activate=true
```

* 8. Execute now

```sh
appwrite functions createExecution --functionId=collect-md-function
```

## GitHub Docs for Dummies

### Sources

* __GitHub Training Manual__ - <https://githubtraining.github.io/training-manual/book.pdf> <br><br>
* __Git and GitHub__ - <https://cs50.harvard.edu/hls/2020/winter/seminars/Git%20and%20GitHub.pdf> <br><br>
* __Git Documentation__ - <https://git-scm.com/> <br><br>
* __Controlling Your Python Projects With Git and GitHub__ - <https://www.pythonguis.com/tutorials/git-github-python/><br><br>
* __Using GitHub with Python__ - <https://pieriantraining.com/using-github-with-python-a-stepbystep-guide/><br><br>
* __How to Use Github API in Python__ - <https://thepythoncode.com/article/using-github-api-in-python><br><br>
* __Working with Repositories__ - <https://www.devdungeon.com/content/working-git-repositories-python><br><br>

## 1. Terminology

* __Branch:__ <br>
a version of the codebase that diverges from the main branch to isolate changes for specific features, fixes or experiments.<br><br>
* __Commit:__ <br>
a snapshot of your changes, saved to your local repository. Each commit is uniquely identified by a checksum.<br><br>
* __Stage:__ <br>
the area where Git tracks changes that are ready to be included in the next commit. Files in the staging area are prepared (staged) for the next commit.<br><br>
* __Merge:__ <br>
the process of integrating changes from one branch into another, typically the main branch.<br><br>
* __Pull Request:__ <br>
a proposal to merge changes from one branch into another, often used in collaborative environments to review and discuss changes before they are merged.<br><br>
* __Fork:__ <br>
a personal copy of someone else's project that lives on your GitHub account.<br><br>
* __Clone:__ <br>
the act of downloading a repository from a remote source to your local machine.<br><br>
* __Remote:__ <br>
a common repository that all team members use to exchange their changes.<br><br>
* __Origin:__ <br>
the default name Git gives to the server from which you cloned.<br><br>
* __Upstream:__ <br>
the original repository taht was cloned.<br><br>
* __Master:__ the default branch name given to a repository when it is created. In modern practice, it is often replaced with `master`.<br><br>
* __Repository:__ <br>
a storage location where your project lives, containing all the files and revision history.<br><br>
* __Working Directory:__ <br>
the directory on yupr computer where you are making changes to your project.<br><br>
* __Staging Area__ or __Index:__<br>
 area where Git tracks changes that are ready to be commited.<br><br>
* __Head:__ <br>
a reference to the last commit in the currently checked-out branch.<br><br>
* __Checkout:__ <br>
the action of switching from one branch to another or to a specific commit.<br><br>
* __Push:__ <br>
the action of sending your commits to a remote repository.<br><br>
* __Pull:__ <br>
the action of fetching changes from a remote repository and merging them into your current branch.<br><br>
* __Fetch:__ <br>
the action of retrieving updates from a remote repository without merging them into your current branch.

## 1. What is GitHub used for?

* Hosting and sharing your code with others.
* Tracking and assigning issues to maintain an organized workflow.
* Managing pull requests to review and incorporate changes into your project.
* Creating your own website using GitHub Pages, a static site hosting service.
* Collaborating with others on projects, making it an excellent tool for open-source contributions.

## 2. What is Git?

* A free and open-source distributed version control system.
* Allows you to track changes to your code over time.
* Enables you to collaborate with others on the same codebase.
* You can easily revert to a previous version of your code or experiment with new features without affecting the main codebase.
* Provides a record of all changes made to your code, including who made them and when which can be useful for auditing and debugging.

## 3. Common Tasks with Git

* Creating a repository
* Creating a branch
* Making changes to a file
* Staging changes
* Committing changes
* Pushing changes to a remote repository
* Merging changes
* Reverting changes
* Deleting a branch

## 4. Why use GitHub with Python?

* GitHub provides a centralized location for storing Python codes, making it easier to mahage and orgaize projects.
* GitHub enables to work with other developers on the same project, making it easier to collaborate and share ideas.
* GitHub offers a rSange of tools and features, such as pull requests, issue tracking, and code reviews, taht can help streamlining teh development workflow and ensure that the code is high quality.

## 5. Use GitHub API in Python

### First, install the dependencies (Git Bash)

```json
pip3 install PyGithub requests
```

### Get User Data - GET request to a specific URL

```python
import requests # type: ignore
from pprint import pprint

username = "github username" # Enter your github username
# URL to request
url = f"https://api.github.com/users/{username}"
# Make the request and return the json
user_data = requests.get(url).json()
# pretty print JSON data
pprint(user_data)
```

__Output :__

```json
{'avatar_url': 'https://avatars.githubusercontent.com/u/169884755?v=4',
 'bio': None,
 'blog': '',
 'company': None,
 'created_at': '2024-05-15T07:51:04Z',
 'email': None,
 ...
 ...
```

### Check Authentication

```python
from github import Github #type: ignore

token = "personal_access_token" # Enter your personal_access_token

g = Github(token)

user = g.get_user()

print(user.login)
```

__Output :__

```json
username
```

### Get Public Repositories from a User

```python
import base64
from github import Github # type: ignore
from pprint import pprint # type: ignore

username = "username" # Github username
# pygithub object
g = Github("your-github-username", "your-github-password") # Enter your username and password
# get that user by username
user = g.get_user(username)

for repo in user.get_repos():
    print(repo)
```

__Output :__

```json
Repository(full_name="user/test")
```

### Get Information about Public Repositories of a User via Username & Password

```python
import base64
from github import Github # type: ignore
from pprint import pprint # type: ignore

username = "username" # Github username
# pygithub object
g = Github("username", "password") # Enter your username and password
user = g.get_user(username)

# iterate over all public repositories
for repo in user.get_repos():
    print(repo.name)
    print("="*100)

'''
# print repository description
for repo in user.get_repos():
    print("Name:", repo.name)
    print("Description:", repo.description)
    print("="*100)
'''
```

__Output :__

```json
Name: test
Description: None
==============
```

### Get Information about Public Repositories of a User via Username & Personal Access Token

```python
import base64
from github import Github # type: ignore
from pprint import pprint # type: ignore

username = "user" # Github username
g = Github("personal_access_token") # or g = Github("username", "password")
user = g.get_user(username)

for repo in user.get_repos():
    print("Repository Name:", repo.name)
    print("Repository Description:", repo.description)
    print("Repository URL:", repo.html_url)
    print("="*100)
```

__Output :__

```json
Repository Name: test
Repository Description: None
Repository URL: https://github.com/user/test
===========
```

### Get file's content via username and personal_access_token

```python
import requests # type: ignore
import base64
# URL
def get_file_from_github(owner, repo, branch, file_path, token):
    url = f"https://api.github.com/repos/{owner}/{repo}/contents/{file_path}?ref={branch}"
    headers = {
        "Authorization": f"token {token}",
        "Accept": "application/vnd.github.v3.raw"
    }
    response = requests.get(url, headers=headers)
    if response.status_code == 200:
        file_content = response.content.decode('utf-8')
        return file_content
    else:
        raise Exception(f"Error: {response.status_code} - {response.json().get('message', '')}")

owner = "username" # GitHub Username
repo = "Repository" # Repository Name
branch = "Branch"   # Branch Name
file_path = "/file.md"  # Pfad zur Datei im Repository
token = "personal_access_token"  # Personal_access_token
try:
    content = get_file_from_github(owner, repo, branch, file_path, token)
    print("Dateiinhalt:")
    print(content)
except Exception as e:
    print(e)
```

### Extract Private Repositories of a Logged-in User via personal_access_token

```python
from github import Github # type: ignore

token = "personal_access_token" # Enter your personal_access_token

g = Github(token)

user = g.get_user()

# Get all repositories of the user, including private ones
repos = user.get_repos(type="all")

# Filter out public repositories
private_repos = [repo for repo in repos if repo.private]

# Print the names of the private repositories
for repo in private_repos:
    print(f"Name: {repo.name}, URL: {repo.html_url}, Description: {repo.description}")
```

__Output :__

```json
Name: username, URL: https://github.com/..., Description: Config files for my GitHub profile.
Name: Exercise, URL: https://github.com/.../Exercise, Description: None
Name: sample_project, URL: https://github.com/.../sample_project, Description: None
....
....
```

### Search for Repositories by Name

```python
from github import Github #type: ignore

token = "personal_access_token" # Enter your personal_access_token

g = Github(token)

search_query = "what are you searching for"  # Replace with the name you want to search for
repos = g.search_repositories(search_query)

for repo in repos:
    print(f"Name: {repo.name}, URL: {repo.html_url}, Description: {repo.description}") 
```

__Output :__

```json
Name: pythoncode-tutorials, URL: https://github.com/x4nth055/pythoncode-tutorials, Description: The Python Code Tutorials
Name: https-github.com-x4nth055-pythoncode-tutorials, URL: https://github.com/quicklers/https-github.com-x4nth055-pythoncode-tutorials, Description: https://github.com/x4nth055/pythoncode-tutorials
...
```

### Search for Repositories by Programming Language

```python
from github import Github # type: ignore

token = "personal_access_token" # Enter your personal_access_token

g = Github(token)

language = "python"  # Replace with the language you want to search for
repos = g.search_repositories(query=f"language:{language}")

for repo in repos:
    print(f"Name: {repo.name}, URL: {repo.html_url}, Description: {repo.description}")
```

### Delete Private Repository by Name

```python
from github import Github #type: ignore

token = "personal_access_token" # Enter your personal_access_token

g = Github(token)

username = "username" # Enter your username

repo_name = "repository-name" # Enter the repository name 

repo = g.get_user(username).get_repo(repo_name)

repo.delete()
print(f"Repository '{repo_name}' deleted.")
```

__Output :__

```json
Repository 'repository-name' deleted.
```

### Create File in a Private Repository

```python
from github import Github #type: ignore

token = "personal_access_token" # Enter your personal_access_token

g = Github(token)

username = "username" # Enter your username

repo_name = "repo-name" # Enter the name of the private repository

repo = g.get_user(username).get_repo(repo_name)

repo.create_file("new_file.txt", "Initial commit", "This is the content of the new file", branch="main")
print("New file created.")
```

__Output :__

```json
New file created.
```

### Delete File from a Repository

```python
from github import Github #type: ignore
from github import Github, GithubException #type: ignore

token = "personal_access_token" # Enter your personal_access_token

g = Github(token)

username = "username" # Enter your username

repo_name = "repo-name" # Enter the name of the repository

repo = g.get_user(username).get_repo(repo_name)

file_path = "file.txt" # Enter the file name 

try:
    file = repo.get_contents(file_path)
    repo.delete_file(file_path, "Delete file", file.sha, branch="main")
    print("File deleted successfully.")
except GithubException as e:
    if e.status == 404:
        print(f"Error: File not found ({file_path})")
    else:
        print(f"Error: {e}")
```

__Output :__

```json
File deleted successfully.
```

### Create Branch

```python
from github import Github # type: ignore

github_token = "personal_access_token" # Enter your personal_access_token
username = "username" # Enter your username

repo_name = "repo-name" # Enter the name of the repository

g = Github(github_token)

repo = g.get_user(username).get_repo(repo_name)

new_branch = repo.create_git_ref(ref="refs/heads/BranchName", sha=repo.get_branch("main").commit.sha) # BranchName - Enter the name of the new branch

print(f"New branch created: {new_branch.ref}")
```

__Output :__

```json
New branch created: refs/heads/BranchName
```

### Delete Branch

```python
from github import Github #type: ignore

github_token = "personal_access_token" # Enter your personal_access_token
username = "username" # Enter your username

repo_name = "repo-name" # Enter the name of the repository

branch_name = "branch-name" # Enter the name of the branch

g = Github(github_token)

repo = g.get_user(username).get_repo(repo_name)

ref = repo.get_git_ref(f"heads/{branch_name}")

ref.delete()

print(f"Branch deleted: {branch_name}")
```

__Output :__

```json
Branch deleted: branch-name
```

### List Branches

```python
from github import Github #type: ignore

github_token = "personal_access_token" # Enter your personal_access_token

repo_name = "repo-name" # Enter the name of the repository

username = "username" # Enter your username

g = Github(github_token)

repo = g.get_user(username).get_repo(repo_name)

branches = repo.get_branches()

for branch in branches:
    print(f"Branch: {branch.name}")
    print(f"  Commit: {branch.commit.sha}")
    print(f"  Commit Message: {branch.commit.commit.message}")
    print(f"  Commit Date: {branch.commit.commit.author.date}")
    print(f"  Commit Author: {branch.commit.commit.author.name}")
    print(f"  Protected: {branch.protected}")
    print("---------")
    
# or 
# for branch in branches:
# print(branch.name)
```

__Output :__

```json
Branch: first
  Commit: c90871b8094f87aade9242dcc79d628bbd2d7e1a
  Commit Message: Delete file
  Commit Date: 2024-06-05 09:15:43+00:00
  Commit Author: username
  Protected: False
---------
Branch: third
  Commit: c90871b8094f87aade9242dcc79d628bbd2d7e1a
  Commit Message: Delete file
  Commit Date: 2024-06-05 09:15:43+00:00
  Commit Author: username
  Protected: False
---------
```

# Pandoc for Windows

### Installing and Working with Pandoc

#### Sources

__Pandoc User's Guide__ - <https://pandoc.org/MANUAL.html> <br>
<https://pandoc.org/installing.html> <br>
<https://pandoc.org/getting-started.html> <br>
__Pandoc's Markdown__ - <https://allefeld.github.io/nerd-notes/Markdown/A%20writer's%20guide%20to%20Pandoc's%20Markdown.html> <br>
__Pandoc Examples__ - <https://pandoc.org/demos.html> <br>
__Try pandoc online__ - <https://pandoc.org/demos.html> <br>
__Using Pandoc with GitHub Actions__ - <https://github.com/pandoc/pandoc-action-example> <br>

#### About Pandoc

Pandoc is a Haskell library for converting from one markup format to another, and a command-line tool that uses this library. <br>
Pandoc can convert between numerous markup and word processing formats, including, but not limited to, various flavors of _Markdown_, _HTML_, _LaTeX_ and _Word docx_.

## 1. Install

### Go to the website and select the version, that you want to install

<https://docs.npmjs.com/downloading-and-installing-node-js-and-npm>

## 2. Open PowerShell

* __Important Note:__ Pandoc is a command-line tool. There is no graphic user interface. So, to use it, you’ll need to open a terminal window.

### Verify Installation

* Type:

```json
pandoc --version
```

## 3. Changing Directories

* Go to a directory you would like to create a new folder. In the following example, you go to the Documents directory:

```json
cd Documents
```

* The following shows your current directory:

```json
pwd
```

* Create a subdirectory, type:

```json
mkdir pandoc-test
```

* Change to the _pandoc-test_ directory:

```json
cd pandoc-test
```

## 4. Using pandoc as a filter

* In the _PowerShell_, type:

```json
pandoc
Hello *pandoc*!

- one
- two
```

and press _Ctrl-Z_ followed by _Enter_. <br>

You should now see your text converted to HTML:

```json
<p>Hello <em>pandoc</em>!</p>
<ul>
<li>one</li>
<li>two</li>
</ul>
```

__Important Note:__ When pandoc is invoked without specifying any input files, it operates as a “filter,” taking input from the terminal and sending its output back to the terminal. You can use this feature to play around with pandoc. <br>
By default, input is interpreted as pandoc markdown, and output is HTML.

* Converting from _HTML_ to _markdown_
In the _PowerShell_, type:

```json
pandoc -f html -t markdown
```

After that, type:

```json
<p>Hello <em>pandoc</em>!</p>
```

Don't forget pressing _Ctrl-Z_, followed by _Enter_

You should see:

```json
Hello *pandoc*!
```

## 5. Text editor basics

* Create a _md_ file, named __test1.md__ in the directory *pandoc-test. <br>
(I created the _test1.md_ in _Visual Studio Code_)

* Type the following in the _test1.md_ and save the file:

```json
---
title: Test
...

# Test!

This is a test of *pandoc*.

- list one
- list two
```

* In the terminal, type `dir` and you should see a list of the current directory, and the file you have already created.

* To convert it to _HTML_, use the command:

```json
pandoc test1.md -f markdown -t html -s -o test1.html
```

* In the directory, you can see the converted _html_ file. Type `.\test1.html` to open it in the browser and see the document.

![No Image to show](assets/pandoc.png)

__Important Note:__ If you get the error `error: This document format requires a nonempty <title> element.
 Defaulting to 'pandoc' as the title.
 To specify a title, use 'title' in metadata or --metadata title="...".` , it indicates that the markdown document does not have a title element. <br>
 To resolve this issue, add a title to the markdown document:

 ```json
 pandoc test1.md -f markdown -t html -s --metadata title="My Title" -o test1.html
 ```

Replace __My Title__ with the desired title for the HTML document.

## 6. Pandoc's Markdown

Pandoc understands an extended and slightly revised version of John Gruber’s Markdown syntax.

## 7. Markdown Extensions

The behavior of some of the readers and writers can be adjusted by enabling or disabling various extensions.

* An extension can be enabled by adding `+EXTENSION` to the format name and
* disabled by adding `-EXTENSION`. <br>

For example: <br>
`--from markdown_strict+footnotes` - is strict Markdown with footnotes enabled <br>
`--from markdown-footnotes-pipe_tables`-  is pandoc’s Markdown without footnotes or pipe tables.

# Data Collector and Appwrite Functions

## Creating and Deploying Functions Using the CLI

* 1. Log in to Appwrite CLI

```sh
appwrite login
```

* 2. Navigate to the project directory

```sh
cd C:\python_files\Git\synergy-rnd-tools\prototyping\appwrite-py\collect-md-function
```

* 3. Prepare the function files

```sh
copy C:\path\to\your\function.py main.py
```

* 4. Create the zip file

```sh
powershell Compress-Archive -Path .\* -DestinationPath .\collect-md-function.zip
```

* 5. Initialize the project (if not already initialized)

```sh
ayppwrite init project
```

* 6. Create the function

```sh
appwrite functions create --functionId "New" --name "New" --runtime python-3.9 --entrypoint="main.py" --execute="any"
```

* 7. Create the deployment

```sh
appwrite functions createDeployment ^ --functionId=Hllo ^ --entrypoint='main.py' ^ --code="HiWorld.zip" ^ --activate=true
```

* 8. Execute now

```sh
appwrite functions createExecution --functionId=collect-md-function
```

## Manually Create, Deploy and Execute a Function in Appwrite

## <https://appwrite.io/docs/products/functions/deploy-manually>

### Create the Function

* Create Manually - Create Function
* Enter Name and Select Runtime
* Upload a File (tar.gz)
* Entrypoint (must be in this case src/main.py)
function.tar.gz
-- src
   -- main.py
-- appwrite.json
-- requirements.txt
* Add a Role (for example Any)
* The Function is created with a status Active, but there are few more settings that must be updated.
* Go to the Function Settings
* Configuration --> __Build Settings__ (if it is empty, you should add the following installation)

```json
pip install -r requirements.txt
```

* Add Environment Variables
* Edit the Timeout (It is automatically set up to 15 sec, Normally the maximum is 900 sec.)
* Redeploy the function
* Exetute Now

* For CMD - Use the following link for init and deploy commands: </n>
<https://appwrite.io/docs/tooling/command-line/deployment#deploying-function>

## Function Template - Depending on the Appwrite Version

### <https://appwrite.io/docs/products/functions/develop>

### <https://appwrite.io/docs/products/functions/examples>

* Appwrite functions receive a context object that provides methods for logging and returning responses (context.log, context.error, context.res.json). This object is specific to the Appwrite runtime environment and is not available in a typical local development setup.

* * Logging:

__context.log(message: str)__: This method is used to log information during the execution of the function. These logs can help with debugging and tracking the function's behavior.
context.error(message: str): This method is used to log error messages. It's particularly useful for capturing and reporting issues that occur during the function's execution.

* * Response Handling:

__context.res.json(response: dict, status_code: int = 200)__: This method is used to return a JSON response from the function. It takes a dictionary as the response body and an optional status code (default is 200, indicating success).

* * Function Execution Details:

The context object may also provide access to details about the execution environment, such as request parameters, headers, and other metadata, although these are not explicitly shown in the provided code.

* The code relies on the context object provided by the Appwrite function runtime. This object is used for logging and responding to function calls, and it's not available in a local development environment.
* The code uses Appwrite-specific services (Client, Databases, Storage) which require an Appwrite server to interact with. These services are not available on your local machine without setting up an Appwrite instance.

```python
import requests
import json
from datetime import datetime, timezone, timedelta
from appwrite.client import Client
from appwrite.services.databases import Databases
from appwrite.services.storage import Storage
from appwrite.input_file import InputFile
from io import BytesIO
import os
from dotenv import load_dotenv


def main(context):
    try:
        # Load environment variables
        load_dotenv()
        appwrite_api_key = os.getenv('APPWRITE_API_KEY')
        some_other_token = os.getenv('SOME_OTHER_TOKEN')

        context.log("Environment variables loaded.")

        # Appwrite configuration
        appwrite_endpoint = 'http://YOUR_APPWRITE_ENDPOINT/v1'
        appwrite_project_id = 'YOUR_PROJECT_ID'
        database_id = 'YOUR_DATABASE_ID'
        collection_id = 'YOUR_COLLECTION_ID'
        bucket_id = 'YOUR_BUCKET_ID'

        # Setup Appwrite client
        context.log("Setting up Appwrite client.")
        client = Client()
        client.set_endpoint(appwrite_endpoint).set_project(appwrite_project_id).set_key(appwrite_api_key)

        databases = Databases(client)
        storage = Storage(client)

        # Define helper functions
        def helper_function_1():
            # Your helper function code
            pass

        def helper_function_2():
            # Your helper function code
            pass

        # Main logic
        context.log("Executing main logic.")
        
        # Example: Fetch data from an external API
        def fetch_data():
            url = 'https://api.external-service.com/data'
            headers = {
                'Authorization': f'Bearer {some_other_token}'
            }
            response = requests.get(url, headers=headers)
            response.raise_for_status()
            return response.json()
        
        data = fetch_data()

        # Example: Process and upload data to Appwrite
        def process_and_upload(data):
            # Process data
            processed_data = data  # Modify this as needed

            # Upload to storage
            file_content = json.dumps(processed_data).encode('utf-8')
            input_file = InputFile.from_bytes(file_content, 'data.json', 'application/json')
            storage.create_file(bucket_id, 'unique()', input_file)

            # Save metadata to database
            document_data = {
                "Filename": "data.json",
                "BucketId": bucket_id,
                "Content": file_content[:32000].decode('utf-8'),  # Assuming content length limit
                "Uploaded": datetime.now(timezone.utc).strftime('%Y-%m-%dT%H:%M:%SZ')
            }
            databases.create_document(database_id=database_id, collection_id=collection_id, document_id='unique()', data=document_data)

        process_and_upload(data)

        context.log("Function execution completed successfully.")
        return context.res.json({
            "success": True,
            "message": "Function executed successfully."
        })
        
    except requests.exceptions.RequestException as e:
        context.error(f"Error fetching data: {e}")
        return context.res.json({
            "success": False,
            "message": f"Error fetching data: {e}"
        }, 500)
    except ValueError as e:
        context.error(f"Error processing data: {e}")
        return context.res.json({
            "success": False,
            "message": f"Error processing data: {e}"
        }, 500)
    except Exception as e:
        context.error(f"An unexpected error occurred: {e}")
        return context.res.json({
            "success": False,
            "message": f"An unexpected error occurred: {e}"
        }, 500)
```

# Examples - Code

## Used Appwrite Syntax

## Function to Get All Markdown Files and Save to Bucket "Copy"

```python
import requests
import json
from datetime import datetime, timezone, timedelta
from appwrite.client import Client
from appwrite.services.databases import Databases
from appwrite.services.storage import Storage
from appwrite.input_file import InputFile
from io import BytesIO
import os
from dotenv import load_dotenv

def main(context):
    try:
        load_dotenv()
        appwrite_api_key = os.getenv('APPWRITE_API_KEY')
        github_token = os.getenv('GITHUB_TOKEN')

        context.log("Environment variables loaded.")
        context.log(f"APPWRITE_API_KEY: {appwrite_api_key}")
        context.log(f"GITHUB_TOKEN: {github_token}")

        # Appwrite configuration
        appwrite_endpoint = 'http://bbl2xtf1/v1'
        appwrite_project_id = '6644aff64740e7fe0d59'
        database_id = 'SystemSpec'
        collection_id = 'Copy'
        bucket_id = 'Copy'
        github_repo_owner = 'philips-internal'
        github_repo_name = 'synergy-base'
        github_branch = 'adrvezyuva/docgen'
        docs_path = 'docs'
        github_start_path = f"https://github.com/{github_repo_owner}/{github_repo_name}/tree/{github_branch}/{docs_path}"

        headers = {
            'Authorization': f'token {github_token}'
        }

        def get_commit_date(owner, repo, path, branch):
            url = f'https://api.github.com/repos/{owner}/{repo}/commits?path={path}&per_page=1&sha={branch}'
            response = requests.get(url, headers=headers)
            response.raise_for_status()
            commits = response.json()
            if commits:
                commit_date_str = commits[0]['commit']['author']['date']
                commit_date = datetime.strptime(commit_date_str, '%Y-%m-%dT%H:%M:%SZ')
                commit_date = commit_date + timedelta(hours=2)
                return commit_date.strftime('%Y-%m-%dT%H:%M:%SZ')
            return None

        def get_markdown_files(owner, repo, path, branch):
            url = f'https://api.github.com/repos/{owner}/{repo}/contents/{path}?ref={branch}'
            response = requests.get(url, headers=headers)
            response.raise_for_status()
            files = response.json()
            markdown_files = []
            for file in files:
                if file['type'] == 'file' and file['name'].endswith('.md'):
                    file['size'] = str(file.get('size', '0'))
                    file['commit_date'] = get_commit_date(owner, repo, file['path'], branch)
                    markdown_files.append(file)
                elif file['type'] == 'dir':
                    markdown_files.extend(get_markdown_files(owner, repo, file['path'], branch))
            return markdown_files

        def upload_files(markdown_files, storage, databases, database_id, collection_id, bucket_id):
            for file in markdown_files:
                file_content = requests.get(file['download_url'], headers=headers).content
                file_bytes = BytesIO(file_content)
                input_file = InputFile.from_bytes(file_bytes.getvalue(), file['name'])

                storage.create_file(bucket_id, 'unique()', input_file)

                document_data = {
                    "Path": file['path'],
                    "Filename": file['name'], 
                    "BucketId": bucket_id,
                    "Content": file_content[:32000].decode('utf-8'), 
                    "Uploaded": (datetime.now(timezone.utc) + timedelta(hours=2)).strftime('%Y-%m-%dT%H:%M:%SZ'),
                    "Commited": file.get('commit_date', 'unknown')
                }
                databases.create_document(database_id=database_id, collection_id=collection_id, document_id='unique()', data=document_data)

        def create_tree_like_structure(markdown_files, path):
            tree_like_structure = {}
            for file in markdown_files:
                file_path = file['path']
                file_name = file['name']
                file_content = requests.get(file['download_url'], headers=headers).text
                current_node = tree_like_structure
                for dir in file_path.split('/'):
                    if dir not in current_node:
                        current_node[dir] = {}
                    current_node = current_node[dir]
                current_node[file_name] = file_content
            return tree_like_structure

        context.log("Setting up Appwrite client.")
        client = Client()
        client.set_endpoint(appwrite_endpoint).set_project(appwrite_project_id).set_key(appwrite_api_key)

        databases = Databases(client)
        storage = Storage(client)

        context.log("Fetching markdown files.")
        markdown_files = get_markdown_files(github_repo_owner, github_repo_name, docs_path, github_branch)
        context.log(f"Fetched {len(markdown_files)} markdown files.")

        context.log("Uploading files to Appwrite storage.")
        upload_files(markdown_files, storage, databases, database_id, collection_id, bucket_id)

        context.log("Creating tree-like structure of markdown files.")
        tree_like_structure = create_tree_like_structure(markdown_files, docs_path)

        json_content = json.dumps({
            "metadata": {
                "markdown_file_count": len(markdown_files),
                "created_time": (datetime.now(timezone.utc) + timedelta(hours=2)).strftime('%Y-%m-%dT%H:%M:%SZ'),
                "github_start_path": github_start_path,
            },
            "tree_structure": tree_like_structure
        }, indent=4)

        json_bytes = BytesIO(json_content.encode('utf-8'))
        input_file = InputFile.from_bytes(json_bytes.getvalue(), 'tree_structure.json', 'application/json')
        storage.create_file(bucket_id, 'unique()', input_file)

        document_data = {
            "Path": "tree_structure.json",
            "Content": json_content[:32000],  
            "Filename": "tree_structure.json", 
            "BucketId": bucket_id,
            "Uploaded": (datetime.now(timezone.utc) + timedelta(hours=2)).strftime('%Y-%m-%dT%H:%M:%SZ'),
            "Commited": (datetime.now(timezone.utc) + timedelta(hours=2)).strftime('%Y-%m-%dT%H:%M:%SZ')
        }
        
        databases.create_document(database_id=database_id, collection_id=collection_id, document_id='unique()', data=document_data)

        context.log("Function execution completed successfully.")
        return context.res.json({
            "success": True,
            "message": "Function executed successfully."
        })
        
    except requests.exceptions.RequestException as e:
        context.error(f"Error fetching files from GitHub: {e}")
        return context.res.json({
            "success": False,
            "message": f"Error fetching files from GitHub: {e}"
        }, 500)
    except ValueError as e:
        context.error(f"Error processing response: {e}")
        return context.res.json({
            "success": False,
            "message": f"Error processing response: {e}"
        }, 500)
    except Exception as e:
        context.error(f"An unexpected error occurred: {e}")
        return context.res.json({
            "success": False,
            "message": f"An unexpected error occurred: {e}"
        }, 500)

```

## Function to Get All MD, SVG, PNG Files saved in different Buckets

```python
# Save MD Files in Bucket "Copy"
# Save Images in Bucket "Images"

import requests
import json
from datetime import datetime, timezone, timedelta
from appwrite.client import Client
from appwrite.services.databases import Databases
from appwrite.services.storage import Storage
from appwrite.input_file import InputFile
from io import BytesIO
import os
from dotenv import load_dotenv

# This is your Appwrite function
# It's executed each time we get a request
def main(context):
    try:
        load_dotenv()
        appwrite_api_key = os.getenv('APPWRITE_API_KEY')
        github_token = os.getenv('GITHUB_TOKEN')

        context.log("Environment variables loaded.")
        context.log(f"APPWRITE_API_KEY: {appwrite_api_key}")
        context.log(f"GITHUB_TOKEN: {github_token}")

        # Appwrite configuration
        appwrite_endpoint = 'http://bbl2xtf1/v1'
        appwrite_project_id = '6644aff64740e7fe0d59'
        database_id = 'SystemSpec'
        collection_id = 'Copy'
        copy_bucket_id = 'Copy'
        images_bucket_id = 'Images'
        github_repo_owner = 'philips-internal'
        github_repo_name = 'synergy-base'
        github_branch = 'adrvezyuva/docgen'
        docs_path = 'docs'
        github_start_path = f"https://github.com/{github_repo_owner}/{github_repo_name}/tree/{github_branch}/{docs_path}"

        headers = {
            'Authorization': f'token {github_token}'
        }

        def get_commit_date(owner, repo, path, branch):
            url = f'https://api.github.com/repos/{owner}/{repo}/commits?path={path}&per_page=1&sha={branch}'
            response = requests.get(url, headers=headers)
            response.raise_for_status()
            commits = response.json()
            if commits:
                commit_date_str = commits[0]['commit']['author']['date']
                commit_date = datetime.strptime(commit_date_str, '%Y-%m-%dT%H:%M:%SZ')
                commit_date = commit_date + timedelta(hours=2)
                return commit_date.strftime('%Y-%m-%dT%H:%M:%SZ')
            return None

        def get_files(owner, repo, path, branch):
            url = f'https://api.github.com/repos/{owner}/{repo}/contents/{path}?ref={branch}'
            response = requests.get(url, headers=headers)
            response.raise_for_status()
            files = response.json()
            relevant_files = []
            for file in files:
                if file['type'] == 'file' and file['name'].lower().endswith(('.md', '.png', '.svg')):
                    file['size'] = str(file.get('size', '0'))
                    file['commit_date'] = get_commit_date(owner, repo, file['path'], branch)
                    relevant_files.append(file)
                elif file['type'] == 'dir':
                    relevant_files.extend(get_files(owner, repo, file['path'], branch))
            return relevant_files

        def upload_files(files, storage, databases, database_id, collection_id, copy_bucket_id, images_bucket_id):
            for file in files:
                file_content = requests.get(file['download_url'], headers=headers).content
                file_bytes = BytesIO(file_content)
                input_file = InputFile.from_bytes(file_bytes.getvalue(), file['name'])
                
                # Determine bucket ID based on file extension
                if file['name'].lower().endswith(('.png', '.svg')):
                    bucket_id = images_bucket_id
                else:
                    bucket_id = copy_bucket_id

                # Upload to the appropriate storage bucket
                storage.create_file(bucket_id, 'unique()', input_file)

                # Create a document in the collection for all file types
                document_data = {
                    "Path": file['path'],
                    "Filename": file['name'], 
                    "BucketId": bucket_id,
                    "Content": file_content[:32000].decode('utf-8', 'ignore'), 
                    "Uploaded": (datetime.now(timezone.utc) + timedelta(hours=2)).strftime('%Y-%m-%dT%H:%M:%SZ'),
                    "Commited": file.get('commit_date', 'unknown')
                }
                databases.create_document(database_id=database_id, collection_id=collection_id, document_id='unique()', data=document_data)

        def create_tree_like_structure(files, path):
            tree_like_structure = {}
            for file in files:
                if file['name'].lower().endswith('.md'):
                    file_path = file['path']
                    file_name = file['name']
                    file_content = requests.get(file['download_url'], headers=headers).text
                    current_node = tree_like_structure
                    for dir in file_path.split('/'):
                        if dir not in current_node:
                            current_node[dir] = {}
                        current_node = current_node[dir]
                    current_node[file_name] = file_content
            return tree_like_structure

        context.log("Setting up Appwrite client.")
        client = Client()
        client.set_endpoint(appwrite_endpoint).set_project(appwrite_project_id).set_key(appwrite_api_key)

        databases = Databases(client)
        storage = Storage(client)

        context.log("Fetching relevant files.")
        relevant_files = get_files(github_repo_owner, github_repo_name, docs_path, github_branch)
        context.log(f"Fetched {len(relevant_files)} relevant files.")

        context.log("Uploading files to Appwrite storage and creating documents.")
        upload_files(relevant_files, storage, databases, database_id, collection_id, copy_bucket_id, images_bucket_id)

        context.log("Creating tree-like structure of markdown files.")
        tree_like_structure = create_tree_like_structure(relevant_files, docs_path)

        json_content = json.dumps({
            "metadata": {
                "markdown_file_count": len([f for f in relevant_files if f['name'].lower().endswith('.md')]),
                "created_time": (datetime.now(timezone.utc) + timedelta(hours=2)).strftime('%Y-%m-%dT%H:%M:%SZ'),
                "github_start_path": github_start_path,
            },
            "tree_structure": tree_like_structure
        }, indent=4)

        json_bytes = BytesIO(json_content.encode('utf-8'))
        input_file = InputFile.from_bytes(json_bytes.getvalue(), 'tree_structure.json', 'application/json')
        storage.create_file(copy_bucket_id, 'unique()', input_file)

        document_data = {
            "Path": "tree_structure.json",
            "Content": json_content[:32000],  
            "Filename": "tree_structure.json", 
            "BucketId": copy_bucket_id,
            "Uploaded": (datetime.now(timezone.utc) + timedelta(hours=2)).strftime('%Y-%m-%dT%H:%M:%SZ'),
            "Commited": (datetime.now(timezone.utc) + timedelta(hours=2)).strftime('%Y-%m-%dT%H:%M:%SZ')
        }
        
        databases.create_document(database_id=database_id, collection_id=collection_id, document_id='unique()', data=document_data)

        context.log("Function execution completed successfully.")
        return context.res.json({
            "success": True,
            "message": "Function executed successfully."
        })
        
    except requests.exceptions.RequestException as e:
        context.error(f"Error fetching files from GitHub: {e}")
        return context.res.json({
            "success": False,
            "message": f"Error fetching files from GitHub: {e}"
        }, 500)
    except ValueError as e:
        context.error(f"Error processing response: {e}")
        return context.res.json({
            "success": False,
            "message": f"Error processing response: {e}"
        }, 500)
    except Exception as e:
        context.error(f"An unexpected error occurred: {e}")
        return context.res.json({
            "success": False,
            "message": f"An unexpected error occurred: {e}"
        }, 500)
```

# Backlog

## Get files' content

```python
import requests #type: ignore
import json

def get_markdown_as_json(owner, repo, branch, file_path, token):
    """Fetch Markdown file from GitHub and return it as JSON."""
    url = f"https://api.github.com/repos/{owner}/{repo}/contents/{file_path}?ref={branch}"
    headers = {
        "Authorization": f"token {token}",
        "Accept": "application/vnd.github.v3.raw"  # Ensure the content is returned as raw data
    }
    response = requests.get(url, headers=headers)
    if response.status_code == 200:
        return json.dumps({"content": response.text})
    else:
        raise Exception(f"Error: {response.status_code} - {response.json().get('message', '')}")

# GitHub details
owner = "adrvezyuva"  
repo = "Exercise" 
branch = "main"  
file_path = "README.md" 
token = "rrrrrrrrrrrrrrrrrrrrr"  

try:
    markdown_json = get_markdown_as_json(owner, repo, branch, file_path, token)
    print("Markdown content:")
    print(markdown_json)
except Exception as e:
    print(e)
```

## Get files' content as a tree with type, level and content

```python
import requests #type: ignore
import json
from bs4 import BeautifulSoup #type: ignore
import markdown #type: ignore

def parse_markdown_to_tree(html_content):
    """ Parses HTML converted from markdown to a tree structure."""
    soup = BeautifulSoup(html_content, 'html.parser')
    tree = []
    current_node = None
    stack = []

    for element in soup:
        if element.name in ['h1', 'h2', 'h3', 'h4', 'h5', 'h6']:
            level = int(element.name[1])  # Level based on header tag
            node = {'level': level, 'type': 'header', 'content': element.get_text(), 'children': []}

            while stack and stack[-1]['level'] >= level:
                stack.pop()

            if stack:
                stack[-1]['children'].append(node)
            else:
                tree.append(node)

            stack.append(node)
        elif element.name in ['p', 'ul', 'ol']:
            node = {'type': 'text', 'content': element.get_text()}

            if stack:
                stack[-1]['children'].append(node)
            else:
                tree.append(node)

    return tree

def get_markdown_from_github(owner, repo, branch, file_path, token):
    """Fetch Markdown file from GitHub and convert to tree structure."""
    url = f"https://api.github.com/repos/{owner}/{repo}/contents/{file_path}?ref={branch}"
    headers = {
        "Authorization": f"token {token}",
        "Accept": "application/vnd.github.v3.raw"
    }
    response = requests.get(url, headers=headers)
    if response.status_code == 200:
        markdown_content = response.text
        html_content = markdown.markdown(markdown_content)
        return parse_markdown_to_tree(html_content)
    else:
        raise Exception(f"Error: {response.status_code} - {response.json().get('message', '')}")

# GitHub details
owner = "adrvezyuva"  # GitHub Username
repo = "Exercise"  # Repository Name
branch = "main"  # Branch Name
file_path = "README.md"  # Path to the file in the repository
token = "rrrrrrrrrrrrrr"  # GitHub Personal Access Access Token

try:
    markdown_tree = get_markdown_from_github(owner, repo, branch, file_path, token)
    print("Markdown Tree:")
    print(json.dumps(markdown_tree, indent=4))
except Exception as e:
    print(e)
```

## Get files' content as a tree with content: and levels

```python
import requests #type: ignore
import json
from bs4 import BeautifulSoup #type: ignore
import markdown  #type: ignore

def parse_markdown_to_tree(html_content):
    """Parses HTML converted from markdown to a tree structure with minimal info."""
    soup = BeautifulSoup(html_content, 'html.parser')
    tree = []
    stack = []  # This stack will keep track of node levels to build the tree

    for element in soup:
        if element.name in ['h1', 'h2', 'h3', 'h4', 'h5', 'h6']:
            node = {'content': element.get_text(), 'children': []}

            while stack and stack[-1]['level'] >= int(element.name[1]):
                stack.pop()

            if stack:
                stack[-1]['children'].append(node)
            else:
                tree.append(node)

            stack.append(node)
            stack[-1]['level'] = int(element.name[1])  
        elif element.name in ['p', 'ul', 'ol']:
            # Create simple text nodes
            node = {'content': element.get_text()}

            if stack:
                stack[-1]['children'].append(node)
            else:
                tree.append(node)

    return tree

def get_markdown_from_github(owner, repo, branch, file_path, token):
    """Fetch Markdown file from GitHub and convert to tree structure."""
    url = f"https://api.github.com/repos/{owner}/{repo}/contents/{file_path}?ref={branch}"
    headers = {
        "Authorization": f"token {token}",
        "Accept": "application/vnd.github.v3.raw"
    }
    response = requests.get(url, headers=headers)
    if response.status_code == 200:
        markdown_content = response.text
        html_content = markdown.markdown(markdown_content)
        return parse_markdown_to_tree(html_content)
    else:
        raise Exception(f"Error: {response.status_code} - {response.json().get('message', '')}")

owner = "adrvezyuva"  
repo = "Exercise"  
branch = "main" 
file_path = "functions.md" 
token = "rrrrrrrrrrrrr"  

try:
    markdown_tree = get_markdown_from_github(owner, repo, branch, file_path, token)
    print("Markdown Tree:")
    print(json.dumps(markdown_tree, indent=4))
except Exception as e:
    print(e)
```

## Get the content of more files at once and save data to JSON file - public repo

```python
import os
import requests #type: ignore
import json
from bs4 import BeautifulSoup #type: ignore
import markdown #type: ignore

def fetch_files(owner, repo, token, path=''):
    """Fetch all Markdown files from a GitHub repository recursively."""
    url = f"https://api.github.com/repos/{owner}/{repo}/contents/{path}"
    headers = {
        "Authorization": f"Bearer {token}",
        "Accept": "application/vnd.github.v3.raw"
    }
    response = requests.get(url, headers=headers)
    if response.status_code != 200:
        raise Exception(f"Failed to list files: {response.status_code} - {response.json().get('message', 'No error message available')}")
    files = response.json()
    markdown_files = []

    for file in files:
        if file['type'] == 'file' and file['name'].endswith('.md'):
            markdown_files.append(file['path'])
        elif file['type'] == 'dir':
            markdown_files.extend(fetch_files(owner, repo, token, file['path']))

    return markdown_files

def fetch_and_parse_md(owner, repo, token, file_path):
    """Fetch a Markdown file and parse it to a tree structure."""
    url = f"https://api.github.com/repos/{owner}/{repo}/contents/{file_path}"
    headers = {
        "Authorization": f"Bearer {token}",
        "Accept": "application/vnd.github.v3.raw"
    }
    response = requests.get(url, headers=headers)
    if response.status_code == 200:
        markdown_content = response.text
        html_content = markdown.markdown(markdown_content)
        soup = BeautifulSoup(html_content, 'html.parser')
        return parse_html_to_tree(soup)
    else:
        raise Exception(f"Error fetching Markdown file: {response.status_code} - {response.json().get('message', '')}")

def parse_html_to_tree(soup):
    """Parse HTML content to a structured tree based on headers."""
    tree = []
    stack = []
    elements = soup.find_all(True)  

    for element in elements:
        if element.name in ['h1', 'h2', 'h3', 'h4', 'h5', 'h6']:
            node = {'content': element.get_text(strip=True), 'children': []}
            level = int(element.name[1])
            while stack and stack[-1]['level'] >= level:
                stack.pop()
            if stack:
                stack[-1]['children'].append(node)
            else:
                tree.append(node)
            stack.append(node)
            stack[-1]['level'] = level
        elif element.name in ['p', 'ul', 'ol']:
            node = {'content': element.get_text(strip=True)}
            if stack:
                stack[-1]['children'].append(node)

    return tree

def check_write_permissions(path):
    """Check if the script has write permissions to the specified directory."""
    if os.access(path, os.W_OK):
        print(f"Write permission is granted for the directory: {path}")
    else:
        print(f"Write permission is NOT granted for the directory: {path}")

def main():
    owner = 'adrvezyuva'
    repo = 'Exercise'
    token = 'rrrrrrrrrrrrrrrrrrrrr'
    markdown_files = fetch_files(owner, repo, token)

    all_trees = {}
    for file_path in markdown_files:
        tree = fetch_and_parse_md(owner, repo, token, file_path)
        all_trees[file_path] = tree

    output_dir = os.getcwd()  
    check_write_permissions(output_dir)
    output_path = os.path.join(output_dir, 'markdown_trees.json')
    with open(output_path, 'w') as file:
        json.dump(all_trees, file, indent=4)
    print(f"JSON file saved to: {output_path}")

if __name__ == "__main__":
    main()
```

## Get the content of more files at once and save data to JSON file - private repo

```python
import os
import requests #type: ignore
import json
from bs4 import BeautifulSoup  #type: ignore
import markdown  #type: ignore

def fetch_files(owner, repo, token, path=''):
    """Fetch all Markdown files from a GitHub repository recursively."""
    url = f"https://api.github.com/repos/{owner}/{repo}/contents/{path}"
    headers = {
        "Authorization": f"token {token}",
        "Accept": "application/vnd.github.v3+json"
    }
    response = requests.get(url, headers=headers)
    if response.status_code != 200:
        raise Exception(f"Failed to list files: {response.status_code} - {response.json().get('message', 'No error message available')}")
    files = response.json()
    markdown_files = []

    for file in files:
        if file['type'] == 'file' and file['name'].endswith('.md'):
            markdown_files.append(file['path'])
        elif file['type'] == 'dir':
            markdown_files.extend(fetch_files(owner, repo, token, file['path']))

    return markdown_files

def fetch_and_parse_md(owner, repo, token, file_path):
    """Fetch a Markdown file and parse it to a tree structure."""
    url = f"https://api.github.com/repos/{owner}/{repo}/contents/{file_path}"
    headers = {
        "Authorization": f"token {token}",
        "Accept": "application/vnd.github.v3.raw"
    }
    response = requests.get(url, headers=headers)
    if response.status_code == 200:
        markdown_content = response.text
        html_content = markdown.markdown(markdown_content)
        soup = BeautifulSoup(html_content, 'html.parser')
        return parse_html_to_tree(soup)
    else:
        raise Exception(f"Error fetching Markdown file: {response.status_code} - {response.json().get('message', '')}")

def parse_html_to_tree(soup):
    """Parse HTML content to a structured tree based on headers."""
    tree = []
    stack = []
    elements = soup.find_all(True)  # Find all HTML elements

    for element in elements:
        if element.name in ['h1', 'h2', 'h3', 'h4', 'h5', 'h6']:
            node = {'content': element.get_text(strip=True), 'children': []}
            level = int(element.name[1])
            while stack and stack[-1]['level'] >= level:
                stack.pop()
            if stack:
                stack[-1]['children'].append(node)
            else:
                tree.append(node)
            stack.append(node)
            stack[-1]['level'] = level
        elif element.name in ['p', 'ul', 'ol']:
            node = {'content': element.get_text(strip=True)}
            if stack:
                stack[-1]['children'].append(node)

    return tree

def check_write_permissions(path):
    if os.access(path, os.W_OK):
        print(f"Write permission is granted for the directory: {path}")
    else:
        print(f"Write permission is NOT granted for the directory: {path}")

def main():
    owner = 'adrvezyuva'  
    repo = 'test'  
    token = 'rrrrrrrrrrrrrrrrrrr'     
    markdown_files = fetch_files(owner, repo, token)

    all_trees = {}
    for file_path in markdown_files:
        tree = fetch_and_parse_md(owner, repo, token, file_path)
        all_trees[file_path] = tree

    output_dir = os.getcwd()  
    check_write_permissions(output_dir)
    output_path = os.path.join(output_dir, 'md_public.json')
    with open(output_path, 'w') as file:
        json.dump(all_trees, file, indent=4)
    print(f"JSON file saved to: {output_path}")

if __name__ == "__main__":
    main()
```

## Download Markdown Files

```python
import requests #type: ignore
import os #type: ignore
import base64 #type: ignore

def main():
    owner = 'philips-internal'
    repo = 'synergy-base'
    token = 'rrrrrrrrrrrrrrrrrrrrr'  
    branch = 'tfeister/docgen'
    folder = 'docs'
    output_dir = 'C:/Users/320257217/Desktop/python_files/Downloaded'  

    url = f"https://api.github.com/repos/{owner}/{repo}/contents/{folder}?ref={branch}"
    headers = {
        "Authorization": f"token {token}",
        "Accept": "application/vnd.github.v3+json"
    }
    response = requests.get(url, headers=headers)
    if response.status_code!= 200:
        raise Exception(f"Fehler beim Abrufen der Dateien: {response.status_code} - {response.json().get('message', 'Keine Fehlermeldung verfügbar')}")

    files = response.json()
    markdown_files = [file['name'] for file in files if file['type'] == 'file' and file['name'].endswith('.md')]
    for file in markdown_files:
        file_url = f"https://api.github.com/repos/{owner}/{repo}/contents/{folder}/{file}?ref={branch}"
        response = requests.get(file_url, headers=headers)
        if response.status_code!= 200:
            raise Exception(f"Fehler beim Herunterladen der Datei {file}: {response.status_code} - {response.json().get('message', 'Keine Fehlermeldung verfügbar')}")
        file_content = base64.b64decode(response.json()['content']).decode('utf-8')
        file_path = os.path.join(output_dir, file)
        with open(file_path, 'w', encoding='utf-8') as f:
            f.write(file_content)
        print(f"Datei {file} erfolgreich heruntergeladen")

if __name__ == "__main__":
    main()
```

## Download md files and make a Markdown summary file

```python
import os
import requests #type: ignore
import json

GITHUB_TOKEN = 'rrrrrrrrrrrr'
GITHUB_REPO_OWNER = 'philips-internal'
GITHUB_REPO_NAME = 'synergy-base'
DOCS_PATH = 'docs'
BRANCH = 'tfeister/docgen'
DOWNLOAD_DIR = 'C:\\Users\\320257217\\Desktop\\python_files\\Downloaded'

def fetch_files(owner, repo, token, path='', branch=''):
    """Fetch all Markdown files from a GitHub repository recursively."""
    url = f"https://api.github.com/repos/{owner}/{repo}/contents/{path}?ref={branch}"
    headers = {
        "Authorization": f"token {token}",
        "Accept": "application/vnd.github.v3+json"
    }
    response = requests.get(url, headers=headers)
    if response.status_code != 200:
        raise Exception(f"Failed to list files: {response.status_code} - {response.json().get('message', 'No error message available')}")
    files = response.json()
    markdown_files = []

    for file in files:
        if file['type'] == 'file' and file['name'].endswith('.md'):
            markdown_files.append(file['path'])
        elif file['type'] == 'dir':
            markdown_files.extend(fetch_files(owner, repo, token, file['path'], branch))

    return markdown_files

def download_markdown_file(owner, repo, token, file_path, branch, download_dir):
    url = f"https://api.github.com/repos/{owner}/{repo}/contents/{file_path}?ref={branch}"
    headers = {
        "Authorization": f"token {token}",
        "Accept": "application/vnd.github.v3.raw"
    }
    response = requests.get(url, headers=headers)
    if response.status_code == 200:
        file_content = response.text
        file_path_local = os.path.join(download_dir, file_path)
        os.makedirs(os.path.dirname(file_path_local), exist_ok=True)
        with open(file_path_local, 'w', encoding='utf-8') as file:
            file.write(file_content)
        return file_path_local
    else:
        raise Exception(f"Error downloading Markdown file: {response.status_code} - {response.json().get('message', '')}")

def generate_summary_markdown(markdown_files):
    summary_content = ''
    for file_path in markdown_files:
        with open(file_path, 'r', encoding='utf-8') as file:
            file_content = file.read()
            summary_content += f"### {os.path.basename(file_path)}\n\n"
            summary_content += file_content + '\n\n'
    return summary_content

def main():
    markdown_files = fetch_files(GITHUB_REPO_OWNER, GITHUB_REPO_NAME, GITHUB_TOKEN, DOCS_PATH, BRANCH)

    downloaded_files = []
    for file_path in markdown_files:
        downloaded_file = download_markdown_file(GITHUB_REPO_OWNER, GITHUB_REPO_NAME, GITHUB_TOKEN, file_path, BRANCH, DOWNLOAD_DIR)
        downloaded_files.append(downloaded_file)

    summary_content = generate_summary_markdown(downloaded_files)
    summary_file_path = os.path.join(DOWNLOAD_DIR, 'summary.md')
    with open(summary_file_path, 'w', encoding='utf-8') as file:
        file.write(summary_content)
    print(f"Summary Markdown file saved to: {summary_file_path}")

if __name__ == "__main__":
    main()
```

## Read the content of all Markdown files and save data to JSON summary file

```python
import os
import requests #type: ignore
import json
from bs4 import BeautifulSoup  #type: ignore
import markdown #type: ignore

owner = 'philips-internal'
repo = 'synergy-base'
token = 'rrrrrrrrrrrrrrrrrrrrrrr'  
branch = 'tfeister/docgen'
folder = 'docs'

def fetch_files(owner, repo, token, path=''):
    url = f"https://api.github.com/repos/{owner}/{repo}/contents/{path}"
    headers = {
        "Authorization": f"token {token}",
        "Accept": "application/vnd.github.v3+json"
    }
    response = requests.get(url, headers=headers)
    if response.status_code!= 200:
        raise Exception(f"Failed to list files: {response.status_code} - {response.json().get('message', 'No error message available')}")
    files = response.json()
    markdown_files = []

    for file in files:
        if file['type'] == 'file' and file['name'].endswith('.md'):
            markdown_files.append(file['path'])
        elif file['type'] == 'dir':
            markdown_files.extend(fetch_files(owner, repo, token, file['path']))

    return markdown_files

def fetch_and_parse_md(owner, repo, token, file_path):
    url = f"https://api.github.com/repos/{owner}/{repo}/contents/{file_path}"
    headers = {
        "Authorization": f"token {token}",
        "Accept": "application/vnd.github.v3.raw"
    }
    response = requests.get(url, headers=headers)
    if response.status_code == 200:
        markdown_content = response.text
        html_content = markdown.markdown(markdown_content)
        soup = BeautifulSoup(html_content, 'html.parser')
        return parse_html_to_tree(soup)
    else:
        raise Exception(f"Error fetching Markdown file: {response.status_code} - {response.json().get('message', '')}")

def parse_html_to_tree(soup):
    tree = []
    stack = []
    elements = soup.find_all(True) 

    for element in elements:
        if element.name in ['h1', 'h2', 'h3', 'h4', 'h5', 'h6']:
            node = {'content': element.get_text(strip=True), 'children': []}
            level = int(element.name[1])
            while stack and stack[-1]['level'] >= level:
                stack.pop()
            if stack:
                stack[-1]['children'].append(node)
            else:
                tree.append(node)
            stack.append(node)
            stack[-1]['level'] = level
        elif element.name in ['p', 'ul', 'ol']:
            node = {'content': element.get_text(strip=True)}
            if stack:
                stack[-1]['children'].append(node)

    return tree

def check_write_permissions(path):
    if os.access(path, os.W_OK):
        print(f"Write permission is granted for the directory: {path}")
    else:
        print(f"Write permission is NOT granted for the directory: {path}")

def main():
    markdown_files = fetch_files(owner, repo, token, folder)

    all_trees = {}
    for file_path in markdown_files:
        tree = fetch_and_parse_md(owner, repo, token, file_path)
        all_trees[file_path] = tree

    output_dir = 'C:/Users/320257217/Desktop/python_files/Downloaded'
    check_write_permissions(output_dir)
    output_path = os.path.join(output_dir, 'new-summary.json')
    with open(output_path, 'w') as file:
        json.dump(all_trees, file, indent=4)
    print(f"JSON-Datei gespeichert in: {output_path}")

if __name__ == "__main__":
    main()
```

## Save directly a Summary Markdown file

```python
import os
import requests #type: ignore
import json

GITHUB_TOKEN = 'rrrrrrrrrrrrrrrrrrrrr'
GITHUB_REPO_OWNER = 'philips-internal'
GITHUB_REPO_NAME = 'synergy-base'
DOCS_PATH = 'docs'
BRANCH = 'tfeister/docgen'
DOWNLOAD_DIR = 'C:\\Users\\320257217\\Desktop\\python_files\\Downloaded'

def fetch_files(owner, repo, token, path='', branch=''):
    """Fetch all Markdown files from a GitHub repository recursively."""
    url = f"https://api.github.com/repos/{owner}/{repo}/contents/{path}?ref={branch}"
    headers = {
        "Authorization": f"token {token}",
        "Accept": "application/vnd.github.v3+json"
    }
    response = requests.get(url, headers=headers)
    if response.status_code != 200:
        raise Exception(f"Failed to list files: {response.status_code} - {response.json().get('message', 'No error message available')}")
    files = response.json()
    markdown_files = []

    for file in files:
        if file['type'] == 'file' and file['name'].endswith('.md'):
            markdown_files.append(file['path'])
        elif file['type'] == 'dir':
            markdown_files.extend(fetch_files(owner, repo, token, file['path'], branch))

    return markdown_files

def download_markdown_file(owner, repo, token, file_path, branch, download_dir):
    url = f"https://api.github.com/repos/{owner}/{repo}/contents/{file_path}?ref={branch}"
    headers = {
        "Authorization": f"token {token}",
        "Accept": "application/vnd.github.v3.raw"
    }
    response = requests.get(url, headers=headers)
    if response.status_code == 200:
        file_content = response.text
        file_path_local = os.path.join(download_dir, file_path)
        os.makedirs(os.path.dirname(file_path_local), exist_ok=True)
        with open(file_path_local, 'w', encoding='utf-8') as file:
            file.write(file_content)
        return file_path_local
    else:
        raise Exception(f"Error downloading Markdown file: {response.status_code} - {response.json().get('message', '')}")

def generate_summary_markdown(markdown_files):
    summary_content = ''
    for file_path in markdown_files:
        with open(file_path, 'r', encoding='utf-8') as file:
            file_content = file.read()
            summary_content += f"### {os.path.basename(file_path)}\n\n"
            summary_content += file_content + '\n\n'
    return summary_content

def main():
    markdown_files = fetch_files(GITHUB_REPO_OWNER, GITHUB_REPO_NAME, GITHUB_TOKEN, DOCS_PATH, BRANCH)

    downloaded_files = []
    for file_path in markdown_files:
        downloaded_file = download_markdown_file(GITHUB_REPO_OWNER, GITHUB_REPO_NAME, GITHUB_TOKEN, file_path, BRANCH, DOWNLOAD_DIR)
        downloaded_files.append(downloaded_file)

    summary_content = generate_summary_markdown(downloaded_files)
    summary_file_path = os.path.join(DOWNLOAD_DIR, 'summary.md')
    with open(summary_file_path, 'w', encoding='utf-8') as file:
        file.write(summary_content)
    print(f"Summary Markdown file saved to: {summary_file_path}")

if __name__ == "__main__":
    main()

```

## Create a folder, download all Markdown files there and create a summary Markdown file

```python
import os
import requests #type: ignore

GITHUB_TOKEN = 'rrrrrrrrrrrrrrrrrrrrrr'
GITHUB_REPO_OWNER = 'philips-internal'
GITHUB_REPO_NAME = 'synergy-base'
DOCS_PATH = 'docs'
BRANCH = 'tfeister/docgen'
DOWNLOAD_DIR = 'C:\\Users\\320257217\\Desktop\\python_files\\Downloaded\\Version1'
SUMMARY_FILE = 'summary.md'

def fetch_files(owner, repo, token, path='', branch=''):
    """Fetch all Markdown files from a GitHub repository recursively."""
    url = f"https://api.github.com/repos/{owner}/{repo}/contents/{path}?ref={branch}"
    headers = {
        "Authorization": f"token {token}",
        "Accept": "application/vnd.github.v3+json"
    }
    response = requests.get(url, headers=headers)
    if response.status_code != 200:
        raise Exception(f"Failed to list files: {response.status_code} - {response.json().get('message', 'No error message available')}")
    files = response.json()
    markdown_files = []

    for file in files:
        if file['type'] == 'file' and file['name'].endswith('.md'):
            markdown_files.append(file['path'])
        elif file['type'] == 'dir':
            markdown_files.extend(fetch_files(owner, repo, token, file['path'], branch))

    return markdown_files

def download_markdown_file(owner, repo, token, file_path, branch, download_dir):
    """Download a Markdown file from GitHub to a local directory."""
    url = f"https://api.github.com/repos/{owner}/{repo}/contents/{file_path}?ref={branch}"
    headers = {
        "Authorization": f"token {token}",
        "Accept": "application/vnd.github.v3.raw"
    }
    response = requests.get(url, headers=headers)
    if response.status_code == 200:
        file_content = response.text
        file_path_local = os.path.join(download_dir, file_path)
        os.makedirs(os.path.dirname(file_path_local), exist_ok=True)
        with open(file_path_local, 'w', encoding='utf-8') as file:
            file.write(file_content)
        return file_path_local
    else:
        raise Exception(f"Error downloading Markdown file: {response.status_code} - {response.json().get('message', '')}")

def generate_summary_markdown(markdown_files, download_dir):
    summary_content = ''
    for file_path in markdown_files:
        file_path_local = os.path.join(download_dir, file_path)
        with open(file_path_local, 'r', encoding='utf-8') as file:
            file_content = file.read()
            summary_content += f"### {os.path.basename(file_path)}\n\n"
            summary_content += file_content + '\n\n'
    return summary_content

def main():
    markdown_files = fetch_files(GITHUB_REPO_OWNER, GITHUB_REPO_NAME, GITHUB_TOKEN, DOCS_PATH, BRANCH)

    for file_path in markdown_files:
        download_markdown_file(GITHUB_REPO_OWNER, GITHUB_REPO_NAME, GITHUB_TOKEN, file_path, BRANCH, DOWNLOAD_DIR)

    summary_content = generate_summary_markdown(markdown_files, DOWNLOAD_DIR)
    with open(os.path.join(DOWNLOAD_DIR, SUMMARY_FILE), 'w', encoding='utf-8') as file:
        file.write(summary_content)

    print(f"Summary Markdown file generated: {os.path.join(DOWNLOAD_DIR, SUMMARY_FILE)}")

if __name__ == "__main__":
    main()
```
