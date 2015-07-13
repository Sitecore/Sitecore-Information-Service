# Sitecore Information Service (SIS)

SIS is a shared system for different Sitecore applications such as 
[SIM](https://github.com/sitecore/sitecore-instance-manager), Config Builder, Diagnostics Tool and others. 

## Intro

It is represented by major parts:

* Definitions - this repository holds the definitions of all supported* Sitecore products
* Updater - the app that generates **Extended Metadata** for Definitions such as configuration files, assemblies versions etc.
* Data - the combination of Definitions and Extended Metadata
* Store - physical location where Data is stored and publicly available by address: http://dl.sitecore.net/updater/info
* Client - .NET assemblies (and NuGet packages) that give easy to use interface to use Store

### Definitions

The definitions are maintained by PSS engineers who are also contact points in the approprate Sitecore product. 
The current state of the Definitions repository does not represent current state of Store because Data generation 
process may take up to 48 hours to complete. [Read more](#definitions-details)

### Updater and Client

The updater app and Client assemblies are maintained by [Diagnostics Toolset](https://github.com/sitecore/sitecore-diagnostics-toolset) 
team.

### Data

The data consists of two pieces:

* Definitions converted into v3 format
* Extended Metadata generated for each of Definitions

## Details

### Definitions Details

Definitions consists of:

* Products
  * Name
  * Metadata (Alias, Default NuGet)
  * Versions
    * Version
    * Revisions

Revision consists of:

* Releases 
  * Release Name (each release must contain "default", but can also have "solr", "delivery", "sdk" etc.)
  * Update Number (e.g. "7")
  * Label (e.g. Initial Release, or Update-7, or Service Pack-2)
  * Release Date
  * Downloads of Distribution Packages
    * Links to SDN or DEV portals
    * Path (name of downloaded zip file plus *optional* path in this zip if necessary)
  * (Optional) Compatibility with other products versions or version and revision(s).
  
Comptibility is defined as

* Outer Dictionary with compatible product name as Keys and Inner Dictionary as Values
* Inner Dictionary with compatible version as key and 
  * either compatible revisions,
  * or empty array if all revisions are compatible

### Definitions Format

Currently supported format is "4".

### Definitions Format "4"

Main data file can have any name and all it can contain is `Metadata`:

```json
{
  "Metadata": {
    "Format": "4"
  }
}
```

In the same directory where main data file is stored, there must be folder called `Products`. 

Every product must be represented by 2 items in the `Products` directory:

* the `Products/<product name>` subdirectory
* the `Products/<product name>.json` file with Metadata object:

```json
{
  "Metadata": {
    "Alias": "SA",
    "DefaultPackage": "SA.Sitecore.Azure"
  }
}
```

Every version of the product must be represented by 

* the `Products/<product name>/<product version>` directory

Every revision of the product version must be represented by

* the `Products/<product name>/<product version>/<product revison>.json` file that follows the format according to this example:

```
{
  "Label": "Update-3",
  "Date": "2015-04-30",
  "Update": "3",
  "Distributions": {
    "default": {
      "Downloads": {
        "Link": "https://dev.sitecore.net/~/media/112486E39AE54610875025BE7FAAC63C.ashx",
        "Path": "Email Experience Manager 3.0 rev.150429 NOT SC PACKAGE.zip/Email Experience Manager 3.0 rev. 150429.zip/package.zip",
      }
    },
    "delivery": {
      "Downloads": {
        "Link": "https://dev.sitecore.net/~/media/112486E39AE54610875025BE7FAAC63C.ashx",
        "Path": "Email Experience Manager 3.0 rev.150429 NOT SC PACKAGE.zip/Email Experience Manager - CD 3.0 rev. 150429.zip/package.zip",
      }
    }
  },
  "Compatibility": {
    "Sitecore CMS": {
      "8.0": []
    }
  }
}
```

Email Experience Manager is distrubited as a regular zip file 
(`Email Experience Manager 3.0 rev.150429 NOT SC PACKAGE.zip`) with several inner zip files that serve different purposes. 
The `"default"` distribution here will be `Email Experience Manager 3.0 rev. 150429.zip` which is indended for default 
Standalone Sitecore installation. 

The `"delivery"` distribution has same `"Link"` but different `"Path",` which points to the package for ContentDelivery instance.

This version of EXM is compatible with any 8.0 release, so the empty array is defined. 
