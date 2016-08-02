*****************
Solution Overview
*****************

.. _Ignition-Solutions:

==================
Ignition Solutions
==================

.. _Ignition-sln:

Ignition.sln
------------

.. _Ignition-tds-sln:

Ignition.Tds.sln
----------------

.. _Ignition-Core-Project:

=====================
Ignition.Core Project
=====================

.. _Adding-Additional-Fields-Definitions:

Adding Additional Fields Definitions
------------------------------------

.. _Creating-Your-Page-Structure:

Creating Your Page Structure
----------------------------

.. _Ignition-Data-Project:

=====================
Ignition.Data Project
=====================

.. _Global-Data-Structures:

Global Data Structures
----------------------

.. _Search-Page-Class-Structure:

Setting up the Search Page Class Structure
------------------------------------------

.. _Ignition-Infrastructure-Project:

===============================
Ignition.Infrastructure Project
===============================

.. _Sitecore-Computed-Fields:

Setting up Sitecore Computed Fields
-----------------------------------

.. _Token-Helper-Setup:

Setting up a Token Helper
--------------------------

.. _Ignition-Sc-Project:

===================
Ignition.Sc Project
===================

.. _Components-Folder-Structure:

Components Folder Structure
---------------------------

.. _Map-Models-Interface-to-Sitecore-Template:

Creating the Models Interface to Map to a Sitecore Template
-----------------------------------------------------------

.. _Create-ViewModel-for-Component:

Creating the ViewModel for a Component
--------------------------------------

.. _Using-the-BaseViewModel:

Using the BaseViewModel
^^^^^^^^^^^^^^^^^^^^^^^

.. _Using-the-ViewPath-Property:

Using the ViewPath Property
"""""""""""""""""""""""""""

.. _Creating-a-Custom-ViewModel:


Creating a Custom ViewModel
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _Creating-the-View-for-Component:

Creating the View for a Component
---------------------------------

.. _Experience-Editor-Support:

Ensuring the View Supports the Experience Editor
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _Creating-Experience-Editor-View:

Creating an Experience Editor View
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _Creating-Agent-for-Component:

Creating an Agent for a Component
----------------------------------

.. _Implementing-PopulateModel:

Implementing PopulateModel
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _Accessing-Component-Datasource:

Accessing the Component's Datasource
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _Setup-And-Access-Rendering-Parameters:

Setting up and Accessing Rendering Parameters
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _Create-Rendering-Parameter-Template:

Creating the Rendering Parameter Template
"""""""""""""""""""""""""""""""""""""""""

.. _Code-Rendering-Parameters-Interface:

Coding the Rendering Parameters Interface
"""""""""""""""""""""""""""""""""""""""""

.. _Accessing-Rendering-Parameter-in-Code:

Accessing the Rendering Parameter in Code
"""""""""""""""""""""""""""""""""""""""""

.. _Accessing-Agent-Parameters:

Accessing the Agent Parameters
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _Search-Sitecore-Items:

Performing Searches Against Sitecore Items in Code
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _Constants-Other-Uses:

Other uses for Constants.cs
---------------------------

The Ignition framework has as a standard for using a constants file as opposed to having constants randomly assigned to variables throughout the application.  When a new Ignition project is created, an IgnitionConstants.cs file is added to the Ignition.Sc project within the Presentation folder of the Ignition solution.

Example of the type of constants oriented information which can be stored in the IgnitionConstants.cs file include

* Placeholder Names
* Folder Names
* Item Names
* Tag Names
* Item GUIDs (when needed)
* View Paths (when needed)

This is not an exhaustive list of string options for constants but keeping these constants in one file makes it easier to re-use and to update this information through-out the application when necessary.

**Code Structure of IgnitionConstants.cs File**

The code structure used to define these constants appear like the following… ::

    namespace Ignition.Sc
    {
        public struct IgnitionConstants
        {
            public struct Placeholders
            {
                public struct Layout
                {
                    public const string LayoutContent = "layoutContent";
                    public const string LayoutHead = "layoutHead";
                }

                public struct Content
                {
               	    public const string BlurbList = "blurb";
                }
            }

            public struct News
            {
                public const string NewsDetailBranchId = "{4713D2A4-6E63-4FDB-B463-6C6DA154725E}";
                public const string InTheNewsDetailBranchId = "{4CB843CE-3868-4CAE-B86F-DBDA7217E4A4}";
                public const string InTheNewsTypeTag = "InTheNews";
            }

            public struct CardTagType
            {
                public const string ContentType = "Content";
                public const string TaxonomyTag = "Tag";
            }
        }
    }

Within the IgnitionConstants class, a bunch of C# structs are used to define the level of constants needed to support the application.  An example of this reference is shown below. ::

    using Ignition.Core.Mvc;

    namespace Ignition.Sc.Components.Content
    {
        public class ContentBlurbListViewAgent : Agent<ContentBlurbListViewModel>
        {
            public override void PopulateModel()
            {
                ViewModel.CurrentTag = IgnitionConstants.CardTagType.ContentType;
            }
        }
    }

In this example, the current tag property of the view model is set by pulling a constant from the IgnitionConstants.cs file used to manage those constants where this value is defined.

.. _Create-Controller-for-Component:

Creating the Controller for a Component
----------------------------------------

.. _Using-View-Method:

Using the View<> Method
^^^^^^^^^^^^^^^^^^^^^^^

.. _Using-Only-ViewModel:

Using Only a ViewModel
""""""""""""""""""""""

.. _Using-ViewModel-and-Agent:

Using a ViewModel and Agent
"""""""""""""""""""""""""""

.. _Explicit-View-Location:

Explicit View Location
""""""""""""""""""""""

.. _Ignition-Root-Project:

=====================
Ignition.Root Project
=====================


.. _Sitecore-Configuration:

Sitecore Configuration
----------------------


.. Ignition-Tests-Project:

======================
Ignition.Tests Project
======================
