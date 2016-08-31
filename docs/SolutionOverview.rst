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

.. _Reusing-the-View-Model:

Re-Using the ViewModel
^^^^^^^^^^^^^^^^^^^^^^^
For most situations when the view path for a component is specified in code, it is either implied by its folder location coinciding with its controller call or directly referenced in the view model used by the view.  However, it is sometimes preferable to make a view model as re-useable as possible by multiple Sitecore component views in MVC using the ignition framework.  The view path can be set dynamically through an agent as part of the controller call so that a view model can be used by multiple separate views representing different Sitecore components.

Does that mean that a re-used view model requires the agent to change the view path as part of the controller action result?  No, as long as (a) the folder containing the controller file matches the controller cs file appendix and (b) the action result method names match the name of the views.  Then the View method call would just accept the view model and the file names and folder structure associated with the controller and views would take it from there.  As an example, what is shown below are three different controller/view calls which use the same view model *ContentBlurbViewModel*.  Note that each View call is using the same agent (ContentBlurbAgent) to help populate the ContentBlurbViewModel but each view call can use a different agent if the process used to populate that view model has to be distinct for each view call. ::

    public ActionResult ContentBlurbView()
    {
        return View<ContentBlurbAgent, ContentBlurbViewModel>();
    }

    public ActionResult ContentBlurbViewTwo()
    {
        return View<ContentBlurbAgent, ContentBlurbViewModel>();
    }

    public ActionResult ContentBlurbViewThree()
    {
        return View<ContentBlurbAgent, ContentBlurbViewModel>();
    }

In each case, the view path is not set dynamically in code because the view file name exactly matches the action result controller call and can be found in the root of the Content folder which also contains the ContentController cs file as shown below in the folder structure image.  So in this sense, the structure directly supports how the views can be accessed without requiring additional code.

.. image:: images/ContentFolderContents.jpg
    :scale: 35
    :align: center

However, if there is a need to structure the location of the view files in a different way or if the view name happens not to match the name of the action result in the controller calling it, then setting the view path dynamically in code so the view model can be re-used does become necessary.  The rest of the discussion of this blog will involve a situation where we are not assuming the view names or file locations and the action results which reference them actually match.


**View File Path Referenced through its Agent**

The view path associated with a controller call used in the Ignition framework can be set dynamically by the developer.  This would allow the same view model to be used by many different views using variations of the View method call regardless of the folder structure of the Ignition framework implementation.  An example of this call is shown below. ::

    public ActionResult HeroSelector()
    {

        return View<HeroSelectorAgent, HeroSelectorViewModel>();

    }

If a developer plans to re-use a view model and vary the view path, it will become necessary to create a view agent (please note the *HeroSelectorAgent* reference in the above example).  The agent is necessary because the view file path must be set which we will use the agent to perform.  Setting the view path on some level should be handled by the agent.   This agent must contains a PopulateMethod method which handles any logic which aids in populating the view model used for the view including where to find the view.  An example of the code used within the Populate method to set the view path is shown below.   The *IgnitionConstants.Hero.HeroSelectorView* constant references the view path.  It is encouraged to have constants defined in a constants cs file like IgnitionConstants.cs and then referenced in those files which needs access to those constants as shown. ::

    public override void PopulateModel()
    {
        ViewModel.ViewPath = IgnitionConstants.Hero.HeroSelectorView; 
    }

That is it.  With what you read you can now re-use view models with different views effectively as a part of developing re-usable content in Sitecore during component development.

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

Passing a Query String using the Ignition Framework
"""""""""""""""""""""""""""""""""""""""""""""""""""
The process of taking advantage of the Ignition framework when it comes to building models is to pass data successfully to the view’s agent so it can use its PopulateModel method to transfer the data to view’s model or perform any other logic necessary to properly render the Sitecore component.

Through MVC’s architecture, a query string can be passed as a parameter of the controller’s ActionResult call.  The key is to take that information and pass it to the agent parameter call so that the agent’s PopulateModel method would then have access to the data and perform logic against it.

The query string object can be passed as a whole to the agent through the View method call or as part of a larger agent parameter object designed to pass that data among others to the view agent call.

As an example, let’s take a look at the code below… ::

    public ActionResult Filters(FilterQueryStringParameters querystringParameters)
    {
        return View<FiltersViewAgent, FiltersViewModel>(querystringParameters);
    }

In this example, the query string associated with the URL call which is accessible by this component is captured through the FilterQueryStringParameters class.  The object is then passed to the agent object (FiltersViewAgent) by passing it as a parameter to the View method.  An example of how the agent’s Populate method would access the query string information is shown below. ::

    public override void PopulateModel()
    {
        var queryStringParameters = AgentParameters as FilterQueryStringParameters;
    }

In this example, the passed in View agent parameter is accessed using the AgentParameters object which now contains this data and is casted to the class used to create the object in the controller call (FilterQueryStringParameters in this case).  At this point, the rest of the agent’s code can focus on using this data to complete the behavior of the view.


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
