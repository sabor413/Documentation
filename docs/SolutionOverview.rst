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
Using the Ignition Framework, we have variety of methods in our disposal for setting the view path to find the view.  The methods of setting the view path which will be discussed in more detail will be…

1. Inherit folder structure
2. Within the View Model itself
3. Using the View Agent
4. Using Agent Parameters

**Inherit Folder Structure**

The Ignition framework is based on .NET MVC framework architecture.   Following MVC convention, if the controller .cs file is located within a folder under the Components folder with the same name and the action result controller call matches the name of the view which it is designed to render, then no additional code is required.

As an example, underneath the Components folder there is a content folder which contains the controller named ContentController.cs.  This controller references three views which resides in the same folder.

.. image:: images/ContentFolderContents.jpg
    :scale: 35
    :align: center

These files are contained within this folder structure so when the controller calls are performed the view path does not need to be explicitly stated.  It is inferred from the action result name and folder structure it is held within.  For instance, the controller code which reference these views is shown below.  Note that the controller calls below while calling Ignition’s View method does not reference an actual view outside of the ActionResult name. ::

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

In addition, the view model supporting these Sitecore component views makes no direct reference to a view path used to find the view within the folder structure of the project as shown below. ::

    using Ignition.Core.Mvc;
    using Ignition.Data.Fields;

    namespace Ignition.Sc.Components.Content
    {
        public class ContentBlurbViewModel : BaseViewModel
        {
            public IHeading Heading { get; set; }
            public ISubtitle Subtitle { get; set; }
            public IRichContent1 RichText1 { get; set; }
            public string HeadingPosition { get; set; }
        }
    }

**Within the View Model Itself**

If we look at the BaseViewModel inherited by the view models of the Ignition framework, it contains a property named ViewPath which can be used to dynamically set the file location of the view the model will populate. ::

    using Ignition.Core.Models.Page;

    namespace Ignition.Core.Mvc
    {
        public class BaseViewModel
        {
            public virtual string ViewPath { get; set; }
            public IPage ContextPage { get; set; }
            public string ParentPlaceholderName { get; set; }
            public bool UseWrapper { get; set; }
        }
    }

If the view model, is only going to be used by one view, then the view path can be set within the view model itself.  For instance, if for the ContentBlurbViewModel, the plan is to only use one ContentBlurbView.cshtml file then you can set the view path value within the view model itself.  This is an option when rendering the view doesn’t involve using a view agent.  Also, by coding it directly, the name and location of the view in relation to the model and the controller does not play a role.  Note that the reference to the view path is passed it by using a constant defined within the project and not passing in a string directly. ::

    using Ignition.Core.Mvc;
    using Ignition.Data.Fields;

    namespace Ignition.Sc.Components.Content
    {
        public class ContentBlurbViewModel : BaseViewModel
        {
            public IHeading Heading { get; set; }
            public ISubtitle Subtitle { get; set; }
            public IRichContent1 RichText1 { get; set; }
            public string HeadingPosition { get; set; }

            public override string ViewPath { get; set; } = IgnitionConstants.ContentViews.ContentBlurbView;

        }
    }

**Using the View Agent**

If there is some logic involved with dynamically determining the view path of a view, then the proper place in code to set this would be through the view agent.

Within the view agent you have access to the view model before its content is rendered by the view including the view path which determines which view file to render.  An example of setting the view path using the PopulateMethod method of the view agent code is shown below. ::

    using Ignition.Core.Mvc;

    namespace Ignition.Sc.Components.Content
    {
        public class ContentBlurbAgent : Agent<ContentBlurbViewModel>
        {
            public override void PopulateModel()
            {
                var parameters = RenderingParameters as IContentBlurbParams;

                if (parameters == null) return;
                ViewModel.HeadingPosition = parameters.Position?.StringSetting;

                var datasource = Datasource as IContentBlurb;
                if (datasource == null) return;
                ViewModel.Heading = datasource;
                ViewModel.Subtitle = datasource;
                ViewModel.RichText1 = datasource;

                ViewModel.ViewPath = IgnitionConstants.ContentViews.ContentBlurbView;
            }
        }
    }

Through this PopulateModel method, the developer can either create sophisticated logic with the same agent to set the proper view path based on which component needs to be serviced in Sitecore or for instance, develop a unique agent for each component re-using the same view model where the view path is set through each individual agent.

**Agent Parameters Can Help**

Another mechanism which can be used to create sophisticated logic to determine the proper view path is an agent parameter which can be passed from the controller call to the PopulateModel method of the agent.  The first step would be to create an agent parameter class used to capture information through an object which would be passed to the agent through the controller.  An example is shown below. ::

    namespace Ignition.Sc.Components.Content
    {
        public class ContentBlurbParams
        {
            public string Params1 { get; set; }
            public string Params2 { get; set; }
            public string Params3 { get; set; }
        }
    }

The next step would involve create an instance of the object during the controller call to include data which can be used to drive sophisticated logic within the agent.  An example of that is shown below. ::

    using System.Web.Mvc;
    using Ignition.Core.Mvc;
    using Ignition.Core.Repositories;

    namespace Ignition.Sc.Components.Content
    {
        public class ContentController : IgnitionController
        {
            public ContentController(ItemContext context) : base(context)
            {
            }

            public ActionResult ContentBlurbView()
            {
                return View<ContentBlurbAgent, ContentBlurbViewModel, IContentBlurbParams>(new ContentBlurbParams
                {
                    Params1 = "First Parameter",
                    Params2 = "Second Parameter",
                    Params3 = "Third Parameter"
                });
            }
        }
    }

During the agent’s PopulateModel method call, the agent parameter would be accessed by casting the incoming agent parameter object (AgentParameters) to the class used to define it in the controller as shown below.  At that point, each object property is available and the developer can then use this data to drive the logic of the agent trying to determine what functionality to invoke including which view to render. ::

    using Ignition.Core.Mvc;

    namespace Ignition.Sc.Components.Content
    {
        public class ContentBlurbAgent : Agent<ContentBlurbViewModel>
        {
            public override void PopulateModel()
            {
                var agentParameters = AgentParameters as ContentBlurbParams;
                var FirstParameter = agentParameters?.Params1;            
            }
        }
    }

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
