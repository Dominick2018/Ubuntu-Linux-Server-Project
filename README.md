Full Stack Web Developer - Nanodegree Program - Item Catalog Project
==================================================================

Purpose
-------

This project is a web application that provides a list of items within a
 variety of categories and integrate third party user registration and
 authentication. Authenticated users should have the ability to post, edit,
 and delete their own items.

Features
---------

    - JSON endpoints
    - User authentication and authorization
    - Full CRUD support using SQLAlchemy and Flask
    - Implements OAuth using Google Sign-in API

Database Definition
-------------------

Catalog_Item database has three tables:

* `category` - has id, name, and user_id columns
* `item` - has id, title, description, category_id, and user_id columns
* `user` - has id, name, email, and picture columns

Tools
-----

  You will need the following tools to run the code for this project:

* `Git Bash Terminal` - comes with the Git software and is used to view the
   .py and .md files.
* `VirtualBox` - the software that actually runs the virtual machine.
* `Vagrant` – the software that configures the VM and lets you share files
   between your host computer and the VM's filesystem.
* `Python 2` - the programming language used to write the source code for
   this program

Install Git (Git Bash Terminal)
-------------------------------

1. Download it from [Git](https://git-scm.com/downloads)
2. Install the version for your operating system.

Install Python
----------------

1. Download from [Python](https://www.python.org/downloads/).
2. Install the version for your operating system.

Install VirtualBox
------------------

1. Download it from [VirtualBox](www.virtualbox.org)
2. Install the platform package for your operating system. You do not need
    the extension pack or the SDK.

Install Vagrant
---------------

1. Download from [Vagrant](https://www.vagrantup.com/downloads.html).
2. Install the version for your operating system.
3. Be aware that the3 Installer may ask you to grant network permission to
    Vagrant or make a firewall exception.

Download the Virtual Machine configuration for Virtual Box
----------------------------------------------------------

1. Go to the Git repository from here [fullstack-nanodegree-vm]
    (<http://github.com/udacity/fullstack-nanodegree-vm>).
2. Clone the repository to your computer.

Program Design
--------------

A Python file has been created that contains the flask routes for url paths
 used in the browser used to indicate functions called. These functions
 will be the action for the web application to allow the user to show
 the catalog, show categories, show items in a category, as well as
 create, edit and delete categories or itesm. By executing the Python file
 named `views.py` on the vagrant command prompt, the Catelog Item app will
 run using the categoryitem.db database to store the data for the app.

Program Files in the project (Dominick_Catalog_Item_Project.zip)
-----------------------------------------------------------------

categoryitem.db - the database
client_secrets.json - used for Google Login
lotsofcategoryitems.py - used to prepopulate the database with items
models.py - used to create the database structure
models.pyc - is the compiled models.py file
README.txt - this readme file
views.py - is the core program that runs the web based site

templates (folder)
    catalog.html - displays the categores and newly added items
    catalogJson.html - displays the entire database in Json
    category.html - displays all the items in the specific category
    deleteCategory.html - displays the category delete screen
    deleteItem.html - displays the item delete screen
    editCategory.html - displays the category edit screen
    editItem.html - displays the item edit screen
    header.html - the header to the web app
    item.html - displays the specific item
    login.html - displays the login screen
    main.html - is the main container of the web app
    newCategory.html - displays the the new category screen
    newItem.html - displays the new item screen
    publiccatalog.html - displays the public catalog
    publiccategory.html - displays the public category
    publicitem.html - displays the public item

static (folder)
    blank_user.gif
    styles.css
    top-banner.jpg

Program Execution
-----------------

1. Ensure that VirtualBox, Vagrant, Git Bash and Python 2 software is
    installed.
2. Save `Dominick_Item_Catalog_Project.zip` in the _fullstack-nanodegree-vm\
    vagrant\catalog_ folder and unzip the folder.
3. Open Git Bash.
4. Navigate to the /fullstack-nanodegree-vm/vagrant folder.
5. Type `vagrant up`.  
    This starts Virtual Box and Vagrant. Note: This command will take some
    time to execute.
6. Type `vagrant ssh`.
    Logs into vagrant.
7. Type `cd /vagrant/catalog`.
8. Run the following commands to install needed softare to the run of the
    program:
    a. `sudo pip install werkzeug==0.8.3`
    b. `sudo pip install flask==0.9`
    c. `sudo pip install Flask-Login==0.1.3`
    d. `sudo pip install pyopenssl`
    e. `sudo pip install requests`
9. Type `python models.py` to create the database.
10. Type `python lotsofcategoryitems.py` to populate the database with
    categories and items.
11. Type `python views.py` to execute the source code for this program.
12. Open your favourite Web browser
13. Type in the url, <http://localhost:8000/>.
14. Type Contol-D to exit from vagrant
