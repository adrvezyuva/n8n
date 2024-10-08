## GitHub Docs for Dummies

### Sources

* **GitHub Training Manual** - <https://githubtraining.github.io/training-manual/book.pdf> <br><br>
* **Git and GitHub** - <https://cs50.harvard.edu/hls/2020/winter/seminars/Git%20and%20GitHub.pdf> <br><br>
* **Git Documentation** - <https://git-scm.com/> <br><br>
* **Controlling Your Python Projects With Git and GitHub** - <https://www.pythonguis.com/tutorials/git-github-python/><br><br>
* **Using GitHub with Python** - <https://pieriantraining.com/using-github-with-python-a-stepbystep-guide/><br><br>
* **How to Use Github API in Python** - <https://thepythoncode.com/article/using-github-api-in-python><br><br>
* **Working with Repositories** - <https://www.devdungeon.com/content/working-git-repositories-python><br><br>

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

## 3. Common Tasks with Git:
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

* First, install the dependencies (Git Bash)
```json
$ pip3 install PyGithub requests
```

* Get User Data - GET request to a specific URL

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
* Check Authentication

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

* Get Public Repositories from a User

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

* Get Information about Public Repositories of a User via Username & Password

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

* Get Information about Public Repositories of a User via Username & Personal Access Token

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

* Get file's content via username and personal_access_token

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

* Extract Private Repositories of a Logged-in User via personal_access_token

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

* Search for Repositories by Name

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

* Search for Repositories by Programming Language

```python
from github import Github # type: ignore

token = "personal_access_token" # Enter your personal_access_token

g = Github(token)

language = "python"  # Replace with the language you want to search for
repos = g.search_repositories(query=f"language:{language}")

for repo in repos:
    print(f"Name: {repo.name}, URL: {repo.html_url}, Description: {repo.description}")
```

* Delete Private Repository by Name

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

* Create File in a Private Repository

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

* Delete File from a Repository

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

* Create Branch

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

* Delete Branch
```python

```

__Output :__
```json
New branch created: refs/heads/BranchName
```

* Delete Branch

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

* List Branches
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