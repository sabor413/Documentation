*****
Setup
*****

Information regarding setting up the Ignition framework is provided through the README file associated with the Sitecore Ignition GitHub repository accessible through `this link`_.

.. _this link: https://github.com/sitecoreignition/SitecoreIgnition/

**Application Dependencies**

The Ignition framework is based on an MVC/C# architecture used to support a Sitecore implementation.  There are a couple of back-end and front-end application dependencies which can be used to provide unique functionality as well as ease of implementation.  

The one *required* dependency is **GlassMapper** which is used to define Sitecore item and Experience Editor related functionality in code.

Other dependencies which are not required but are supported include...

* Unicorn or Hedgehog TDS [#f1]_
* Castle Windsor
* Sitecore Powershell
* Bootstrap/jQuery/CSS [#f2]_

Please continue to use the above link for any updates to the installation process concerning the Ignition framework.

.. [#f1] The Ignition framework GIT repository is primarily using Unicorn to synchronize the items which make up the Sitecore implementation.  Hedgehog TDS can be used in its place and there is an implementation involving TDS in the GIT repository.  Neither is required.  Sitecore packages are provided containing the base item structure implementation of this framework.

.. [#f2] Some of the demo components developed may use this front-end functionality.  The JS scripts files to support that functionality is included in the Ignition framework.