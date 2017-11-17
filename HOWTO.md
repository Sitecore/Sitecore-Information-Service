# Sitecore Information Service (SIS)

## Index

**Important!** Read [VERSIONING.md](VERSIONING.md) document first.

* [Intro](#intro)
* [Workflow](#workflow)
* [How To: Add New Product](#how-to-add-new-product)
* [How To: Add New Release](#how-to-add-new-release)

## Intro

In order to perform any changes in the info service definitions, use the following workflow:

### Workflow

* Create `feature/<something>` branch (for example `feature/add-wffm-823-824`)
* Perform necessary changes in `products/<something>` branch
* Create Pull Request from `products/<something>` branch to `master`
* Follow reviewer instructions if provided

As a result:

* Changes will be merged to `master` branch by reviewer
* Within 48 hours changes should be propagated to all clients

## How To: Add New Product

This document contains instruction example of how to add a new product to info service.

### Create Product Metadata File 

```
data/Products/Sitecore Lollipop.json
```

#### Description

This file name is very important - it will be used everywhere as an official name of the product - both as ID, Name 
and Display Name. 

The contents of the file is a metadata for the product:
```json
{
  "Metadata": {
    "Alias": "SL",
    "DefaultPackage": "SL.Sitecore.Lollipop",
  }
}
```

Here `SL` as an alias that will be used as a prefix for all NuGet packages of all module releases. For example:

* SL.Sitecore.Lollipop
* SL.Sitecore.Lollipop.Services
* SL.Sitecore.Lollipop.Client
* etc.

And `DefaultPackage` points to the NuGet package that is installed by default.

## How To: Add New Release

This document contains instruction example of how to add a new release of the already defined product to info service.

### Create Product Release File 

```
data/Products/Sitecore Lollipop/8.2.5.json
```

#### Description

The file path is very important. 

* The `Sitecore Lollipop` folder points to the Product Name defined by Product Metadata File - it must
be exactly the same. 
* The `8.2.5.json` file name points to the Product Version the release will belong to. The `.json` extension is required for the system 
to process the file.

The contents of the file describes the release details, compatibilities map and "release distributions" - different 
packages that belong to release.
```json
{
  "Label": "Initial Release",
  "Date": "2017-02-31",
  "Revision": "170728",
  "Distributions": {
    "default": {
      "Downloads": {
        "Link": "https://dev.sitecore.net/~/media/00000000000000000000000000000000.ashx",
        "Path": "Sitecore Lollipop 8.2 rev. 170728 NOT SC PACKAGE.zip/Sitecore Lollipop 8.2 rev. 170728.zip/package.zip",
      }
    },
    "delivery": {
      "Downloads": {
        "Link": "https://dev.sitecore.net/~/media/11111111111111111111111111111111.ashx",
        "Path": "Sitecore Lollipop 8.2 rev. 170728 for Content Delivery.zip/package.zip",
      }
    }
  },
  "Compatibility": {
    "Sitecore CMS": {
      "8.2": []
    },
    "Email Experience Manager": {
      "3.4.0": []
    }
  }
}
```

Remarks:

* `Initial Release` is a human-readable label of the specific release. It can also be something like `Update-3` 
or `Service Pack-1`.
* `2017-02-31` is a date when the release became publicly available
* `default` distribution is a must-have distribution name. Every release must have it. Others are optional, but 
this is essential - to be used when you don't know which distribution to use.
* `https://dev.sitecore.net/~/media/00000000000000000000000000000000.ashx` is a link where you can download 
the `Sitecore Lollipop 8.2 rev. 170728 NOT SC PACKAGE.zip` file. It is very imporant that the file name exactly 
matches to the default file name of the file downloaded from this link.
* `Sitecore Lollipop 8.2 rev. 170728 NOT SC PACKAGE.zip/Sitecore Lollipop 8.2 rev. 170728.zip/package.zip` means that 
to get the actual release files the build server needs to download 
the `Sitecore Lollipop 8.2 rev. 170728 NOT SC PACKAGE.zip` file, extract it to some temporary folder, find there 
the `Sitecore Lollipop 8.2 rev. 170728.zip` file, extract it to some other temporary folder, find there 
the `package.zip` file and treat it as distribution file to analyze.
* Again, `https://dev.sitecore.net/~/media/11111111111111111111111111111111.ashx` is a link that allows to download 
the `Sitecore Lollipop 8.2 rev. 170728 for Content Delivery.zip` file dedicated for Content-Delivery instances.
* Again, `Sitecore Lollipop 8.2 rev. 170728 for Content Delivery.zip/package.zip` outlines that build server needs 
to unpack the `Sitecore Lollipop 8.2 rev. 170728 for Content Delivery.zip` file to some temporary folder, find there 
the `package.zip` and treat it as distribution file to analyze.
* The `Compatibility` section points out that this release is compatible with any release of Sitecore CMS 8.2, but 
it is only compatible with Email Experience Manager 3.4 Update-0. 
