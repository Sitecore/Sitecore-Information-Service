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
process may take up to 48 hours to complete. [Read more](#definitions-format)

### Updater and Client

The updater app and Client assemblies are maintained by [Diagnostics Toolset](https://github.com/sitecore/sitecore-diagnostics-toolset) 
team.

### Data

The data consists of two pieces:

* Definitions converted into v3 format
* Extended Metadata generated for each of Definitions

## Details

### Definitions Format

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
    * Links to alternative locations (Optional)
    * Filenames (default filename of the downloaded distribution file)
  * Compatibility with other products versions or version and revision(s).
  
Comptibility is defined as

* Outer Dictionary with compatible product name as Keys and Inner Dictionary as Values
* Inner Dictionary with compatible version as key and 
  * either compatible revisions,
  * or empty array if all revisions are compatible
