Welcome to TYPO3 - TYPO3camp Venlo
==================================

This repository is used for the workshop 'Introduction building a website TYPO3' at http://www.typo3campvenlo.nl
Main keypoints of this workshop are:
* Installing TYPO3 by composer
* Installing the TYPO3 introduction package https://typo3.org/extensions/repository/view/introduction
* Adjusting the introduction package styling to your own
* Adding a news systems to your website https://github.com/TYPO3-extensions/news
* Creating a custom 'Blog' extension

TYPO3 System requirements
=========================

TYPO3 is based upon PHP and uses a MySQL database. For more information
regarding these requirements see the [INSTALL](https://github.com/TYPO3/TYPO3.CMS/blob/master/INSTALL.md) file.

Using the Database Abstraction Layer (DBAL) allows one to use TYPO3 with other
Database Management Systems, like PostgreSQL, Oracle and MSSQL.

Installing TYPO3 by composer
============================

Installation of TYPO3 can easily done by [composer][]. A quick way to install TYPO3 can be used by create a project with
the TYPO3 Base distribution:

    composer create-project typo3/cms-base-distribution projectname

More information about how to install TYPO3 by composer can be found at https://composer.typo3.org

[composer]: https://getcomposer.org/ "The PHP package manager"

Setting up the TYPO3 website
----------------------------

The installation of TYPO3 succeeded and now it is time to setup/configure TYPO3. TYPO3 is shipped with a setup tool which performs the necessary steps to full fill the configuration. If you visit your installation now in the browser you see a message thanking you for downloading TYPO3. In here it will also be mentioned to create a file 'FIRST_INSTALL' to continue.

Create a file in your webdir of your installationm on the terminal hit the following command:

    cd web; touch FIRST_INSTALL;

Visit the website again in the browser and walkthrough the onscreen steps in short:

1. System environment check
  Fix corresponding errors and continue
2. Database credentials
  - Create your database in phpmyadmin and make sure you database collation is set on 'utf8_general_ci'
  - Enter the credentials
3. Select your created database
4. Create user and import data
5. Optionally select a preconfigured website
  - For this workshop we select 'Do nothing, just get me to the Backend' because we install the distribution by composer.

Installing the TYPO3 introduction package / other extension
===========================================================

When visiting your website by the url gives you the error 'Service unavailable (503) - No pages are found on the rootlevel'. This is normal because we don't have installed the introduction package neither we have setup a single page we could view. When selecting the option 'Create a empty page' in step 5 of the configuration a single page should have be shown.
The introduction package we are going to install is an TYPO3 extension which we add to our installation. TYPO3 offers a lot of extensions you can see at TYPO3 Extension Repository (TER) at https://typo3.org/extensions/repository/ .
To install a extension just require the extension in your main composer by the followed command  (installing the introduction package):

    composer require typo3-ter/introduction

TYPO3 automatically places this extension in the correct extension directory ('web/typo3conf/ext/') were all third party and own extension will be. (When using a NOT composer based installation you can either download the files and place this into this directory OR use the Backend extensionmanager to download an extension from TER)

Last step is to active the extension (in this case the introduction package) in the Backend (BE) extension manager. When you now visit the website you will have you're first TYPO3 website running!

TYPO3 basics / common practises / structure of your TYPO3 installation
======================================================================

Before going to the next section *Personalize the introduction package* it is good to know some basics / common practises /
structure of your TYPO3 installation.

Definition list
---------------
Below here are some definitions that are quite often used and good to know ;)

Rootpage
	The root of your website (presented with the globe-icon). (In the IP 'Congratulations');
uid
	Unique identifier of a page/item/object
pid
	Parent identifier of the current page/item/object
TER
	The **T** YPO3 **E** xtension **R** epository which contains over 6000+ already available extensions free to use!
BE
    TYPO3 Backend
FE
    TYPO3 Frontend / your website

Structure of the BE
-------------------

The structure of the BE (after login at (*www.yourwebsite.com/***/typo3**)) is as follows:

1. Available backend modules
   - The page and list module you are going to use the most to control the content.
   - The template/extensions/install/configuration is mostly used to configure/control/develop/debug your website
2. Page tree (only visible in module under the category 'Web')
   - The different pages and folders/storages containing the content of your website
3. Content/adjust part
4. Tools for caching/searching and to adjust user settings

![TYPO3 Backend structure] (Images/typo3_backend.png)


Troubleshooting / tips
----------------------

Here are a list of common problems I experienced/experiencing during developing in TYPO3 and afterwards you think 'Of course!!'.

* Did you cleared the caches?!! (clear the caches in the top right cache menu)
* Is my setting really used and not overwritten somewhere?  In other words what is the loaded typoscript setting in the template object browser?
* Is the typoScript not been overwritten/ is it visible?
* Is the database up to date with my code (perform an database check in the install tool)
* Is the extension active / are the dependencies to other extension set in your ext_emconf.php and are those active?
* To be continued with 'AHA moments'

Minimal overview of a website structure in dirs
-----------------------------------------------

Directory of TYPO3 composer based:
```
├── vendor
├── web
│   ├── fileadmin
│   ├── typo3 (symlink to the vendor typo3 folder)
│   ├── typoconf
│   │   ├── ext (The extension directory)
│   │   ├── l10n (the language files downloaded for the BE)
│   │   ├── LocalConfiguration.php (Local configuration of the website e.g. database access)
│   │   ├── PackageStates.php (keeps track of the package/extension states (written by the extensionmanager))
│   ├── typo3temp (cached files)
│   ├── uploads (user uploads)
│   ├── .htaccess
│   ├── index.php (symlink to the vendor typo3 index which is your website startpoint)
├── composer.json
├── composer.lock

```

Commonly used directory structure for an extension

```
├── site_template
│   ├── Classes
│   │   ├── Command (command controllers that can invoke terminal commands)
│   │   ├── Controller (controllers for the FE/BE)
│   │   ├── Domain
│   │   │   ├── Model (Domain model objects)
│   │   │   ├── Repository (Repositories to the database (ORM mappers))
│   │   ├── ViewHelpers (Classes to help within the view)
│   ├── Configuration
│   │   ├── TCA (Table Configuration Array)( Object mapping between DB and model) (also config for the BE lists /edits)
│   │   ├── TypoScript
│   │   │   ├── [OPTIONAL] Other dirs e.g. BeLayouts
│   │   │   ├── constants.txt (typoscript constants)
│   │   │   ├── setup.txt (typoscript setup (includes the other files in the "other dirs")
│   ├── Resources
│   │   ├── Private
│   │   │   ├── Ext
│   │   │   │   ├── News
│   │   │   │   │   ├── Layouts
│   │   │   │   │   ├── Partials
│   │   │   │   │   ├── Templates
│   │   │   ├── Language (contains all languages and translations)
│   │   │   ├── [OPTIONAL] Less
│   │   │   ├── Layouts (The fluid layouts)
│   │   │   ├── Partials (The fluid partials)
│   │   │   ├── Templates (the fluid templates)
│   │   │   ├── .htaccess
│   │   ├── Public
│   │   │   ├── Css 
│   │   │   ├── Icons
│   │   │   ├── Images
│   │   │   ├── Fonts
│   │   │   ├── Javascript
│   ├── ext_icon.png
│   ├── ext_emconf.php
│   ├── ext_localconf.php
│   ├── ext_tables.php
│   ├── ext_tables.sql

```

====================================
Personalize the introduction package
====================================

Personalizing the website to your own preferences have to be done in your OWN extensions and **NOT!!** in thirds party extensions.
 Modifying those means updating those extensions (as bugfixes) is **HARD** and your code is mixed which is a **very bad situation** to have.
If there are bugs then clone the original repository fix it and notify *or even better provide a patch* to the original builder so that the extension can be updated and other can also have profit of your work.

Create a custom extension to adjust the introduction package:
-------------------------------------------------------------

The personalization of the website / introduction package will be done in a own TYPO3 extension 'site_template'. The minimal requirements for an own TYPO3 extension are:

* Directory with the name of the extension (site_template)
* ext_emconf.php

   This file is needed to recognize and activate your extension in the extensionmanager.
   See the file [ext_emconf.php] (/Files/ext_emconf.php)
   
* ext_icon.png (16px*16px) [ext_icon.png] (/Files/ext_icon.png)

If you go to the extension manager in the backend you will see your extension and can activate your extension.

Adjust the logo of the  introduction package
--------------------------------------------

In the introduction pacakge the logo is made configurable by TypoScript, to adjust this logo and logo properties (height/width/alt) we need to override the constants so that the correct file to our logo is used. If you look into the `constants.txt` of the extension bootstrap package you see the following information :

    page {
        logo {
            # cat=bootstrap package: basic/110/100; type=string; label=Logo: Leave blank to use website title from template instead
            file = EXT:bootstrap_package/Resources/Public/Images/BootstrapPackage.png
            # cat=bootstrap package: basic/110/110; type=int+; label=Height: The image will not be resized!
            height = 60
            # cat=bootstrap package: basic/110/120; type=int+; label=Width: The image will not be resized!
            width = 210
            # cat=bootstrap package: basic/110/130; type=string; label=Alternative text: Text of the alt attribute of the logo image (default: "<website title> logo")
            alt =
        }
    }
    
To override this create your own `setup.txt` (for later use) and `constants.txt` (see extension dir structure which directory). Copy those contents as above in the constants.txt and modify the file to the path to your own logo in the site_template extension. For example : `file = EXT:site_template/Resources/Public/Images/logo.png`. The `EXT:site_template` directs to your extension directory.

The TypoScript then needs to be included in your website. This needs a certain order otherwise because we want to override the bootstrap_package constants and not let the bootstrap constants override ours. Therefore we first need to mention it as an *static template* by adding the following lines to `ext_tables.php` . 

    /TYPO3\CMS\Core\Utility\ExtensionManagementUtility::addStaticFile(
		$_EXTKEY,
		'Configuration/TypoScript',
		'Site template (after bootstrap package)'
	);

After this we can include this *static template* to our website by: the following steps
 
- In the BE select Template module (BE structure 1)
- Select rootpage in the pageTree (Be structure 2)
- Select Info/modify (be structure 3 top left)
- Go to 'Edit the whole template record'
- Include your static in the tab 'includes'
- Adjust order that the site_template comes **after** bootstrap_package
- Save changes
- Clear cache

**In the 'info/modify' -> 'Edit the whole template record' TypoScript could be overwritten in the fields 'settings' or 'constants'.
The values can be deleted because we are going to set this in our site_template.**

(another way to navigate to the 'Edit the whole template record is: List Module -> Select rootpage -> edit Template)

The website logo should have been updated now.

####Verify correct override of the TypoScript constant

When you navigate to the Template module, select your root page in the pagetree (BE structure 2) and go to the Select option
'TypoScript Object browser' you can check under constants which settings are used.

Adjust the colors/css
---------------------

The default color scheme / css styling is perhaps not as desired. To adjust this in TYPO3 you can add your own css styles.

First step is to create an css file to adjust the color of the menu bar to the economical green color. This file is called
main.css and is located at Resources/Public/Css. Content of this file to change the menubar to green is:

	.navbar{
		background-color: green;
	}

Second step is to configure/setup TYPO3 to include this file at every page. In the created `setup.txt` file you have to
 add the following line to include your main.css file.

	page.includeCSS.all = EXT:site_template/Resources/Public/Css/main.css

After clearing the cache, your menu bar has an economical greenish background color.

Add news extension to your website
==================================

One commonly used functionality of a website is presenting some news/blog/information presented in a list with a detail view.
In TYPO3 the extension 'news' offers the functionality to present this. In this section it will be explained how to add
this to your website, and how to adjust the view of this.

Installation of news can be done like the introduction package, in short:

* Run composer require `composer require typo3-ter/news`
* Activate the extension in the extensionmanager module

When the extension is installed a plugin can be added to a certain page to show all news records of a storage.
The storage can be the page were the plugin is listed, but it is *recommended* to use a folder/storage to keep your website
a structured. The setup of news could be done for example by:

* Create a new folder/storage in the pagetree
* Edit the folder and look for the field 'use as a container' and select 'news'.
* Create a few news items in this folder (List module, add item -> add news)
* Create two pages in the pagetree  for example 'news' and 'news-detail'
 
**The detail-view can be hide in the menus because user are redirect from the list-view and accessing this page will result in an error because the page does not know what to show. *(You can hide the detail view in the edit page mode.)***

* Create a plugin on the created news page in the pageview where you want to show the news.
   * Select page module
   * Select the desired page, in this case 'news'
   * Add content on the desired place 'Add content'
   * Find and select 'news system' and go the plugin settings
* Configure the plugin as followed:
   * Configure what to display: *select 'listview (without overloading detail view)*
   * Configure where your news items are stored: *Enter/search for your folder/container in the field 'select startingpoint'*
   * Configure where to redirect to for an detail page of the news items: *Enter/search for your detail page at the field 'detail pageId'*
   * Save your changes

Presenting the detail of a news items is done by adding another news plugin to your detail page and configure the
starting point to the same folder as previous and selecting 'detail-page' in the field 'what to display'.

When visiting the website (cleared the cache / set the page on visible?) you should see your news items.

The complete user manual as other information about this extension can be found at:
http://typo3.org/extensions/repository/view/news

Adjusting the view(s) of the news extension
-------------------------------------------

After you installed the news extension you probably also want to adjust the view of the news list/ detail-page to your
needs. To do this a few steps are needed and you can adjust the templates of news in the site_template.

Adjusting the view is own extension is mentioning TYPO3 to first look in your extension to the news templates. If the template is found here this template is used otherwise the template of news is found. (are none of the templates are found you get an exception).

At this point you should already have added the typoscript to your website in the part were you adjusted the logo, if you skipped this have a look in that section.

The TypoScript to inform TYPO3 to look for our own templates add the following to the `setup.txt`

	plugin.tx_news {
		view {
			templateRootPaths {
				102 = EXT:site_template/Resources/Private/Ext/News/Templates/
			}

			partialRootPaths {
				102 = EXT:site_template/Resources/Private/Ext/News/Partials/
			}

			layoutRootPaths {
				102 = EXT:site_template/Resources/Private/Ext/News/Layouts/
			}
		}
	}

*Better practise is to created an folder 'Lib' in the TypoScript folder and create a file for every extension you change the settings for (just to be organized). Move the settings of news to for example the file `Configuration/TypoScript/Lib/news.ts` and include all files in the Lib directory in your setup:*

    # Lib
    <INCLUDE_TYPOSCRIPT: source="DIR:EXT:site_template/Configuration/TypoScript/Lib/">

Adjusting the template is now a matter of creating the same file (or copying the original) in the mentioned paths above.

*When no changes are visible, make sure that the path AND filename relative to the given rootPaths are the same, else the template of news is taken. (you can make a small adjustment in the news template to check if the news template is still used)*

Create your first custom extension
----------------------------------

**The case of the first extension is a simple blog system to create, update, edit, delete blogitem(s) on the website as
well in the backend.
On the website there should be an overview of al blog items as well an detail view of every item.
A blog item consists of a title, teaser, date and a message, where the date will be automatically will be set when creating
the blog item.**

Creating extensions can easily be kick started by the extension builder. The extension builder can be found at TER but
the version compatible for CMS 7 is not yet released in the TER. Therefore use the following link to get it:
https://github.com/TYPO3-extensions/extension_builder.git

The manual of the extension builder can be found at:
http://docs.typo3.org/typo3cms/extensions/extension_builder/

Once you have installed the extensionbuilder we can use the backend module to create our simple_blog extension.

The entered information in the demo is as follows:

- Name: Simple blog
- Vendor name: Yourcompany
- Key: simple_blog
- Descr: My first extension, a nice blogging system
- More options: keep defaults
- Add a person: enter your personal information
- Add a front end plugin:
- Name: Listblog
- Key: list

After this create a 'New Model Object', in this case the Blog object.

- Mark the "Is aggregate root" *This creates a repository and creates the mapping*
- Check the required action that should be possible
- At the properties we want the following:

- title (String)
- date (DateTime(timestamp))
- teaser (String)
- message (Rich text)

Save your extension and install this in the extension manager.

If you go to the typo3conf/ext you will see the created extension simple_blog. Under the folder Classes/Domain/Model the
Blog is created with default getters and setters. In here we create a constructor that sets the date on creation of an item.
The following constructor is created which sets the date::

	/**
	 * Constructor of a new Blog which automatically sets the date on today
	 */
	public function __construct() {
		$this->date = new \DateTime();
	}

The other thing we need to adjust is that a web user does not have to enter the field. In the folder
Resources/private/Partials/Blog/FormFields.html the following has to be removed::

	<label for="date">
	<f:translate key="tx_simpleblog_domain_model_blog.date" />
	</label><br />
	<f:form.textfield property="date"  value="{blog.date->f:format.date()}" /><br />

.. note::
	**The following steps assume that you have already done/read the section about adding the news system, due the fact
	that some steps are already explained over there.**

If this is all setup you have to create a folder/storage for the blog items and create a page containing your plugin.

As different with the news extension in creating a new content element the plugin is under 'General plugin' and after
this go to the tab 'Plugin' and select your plugin. ('ListBlog' or the name you specified before in the extension builder)

At last enter/find the record of your folder/container and set this in the field 'Record Storage Page'.

When you cleared the caches and visit your website you can see your first blog system. In the backend you can also adjust
your blog items in your container or add new ones if desired.
