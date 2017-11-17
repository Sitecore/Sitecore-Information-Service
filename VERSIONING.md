# Sitecore Information Service (SIS)

### Versioning

It is very important to use proper version.
SIS supports only specific format that must be obeyed independently of how product was published.

```
{Major}.{Minor}.{Update}
```

#### Samples

Consistent:  
* `Sitecore CMS 6.5.0` => Sitecore 6.5.0 rev. 110602
* `Sitecore CMS 6.5.1` => Sitecore 6.5.0 rev. 110818
* `Sitecore CMS 8.2.5` => Sitecore 8.2 rev. 170728 

Inconsistent:  
* `Sitecore Commerce 8.2.0` => Sitecore Commerce 8.2.1 Initial Release (January 2017)
* `Sitecore Commerce 8.2.1` => Sitecore Commerce 8.2.1 Update-1 (April 2017)
* `Sitecore Commerce 8.2.2` => Sitecore Commerce 8.2.1 Update-2 (July 2017)

Despite official family version is **8.2.1**, SIS does not support 3-digit format for version familieis so it was shortended to `8.2`.
