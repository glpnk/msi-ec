# Docs and guides

> [!CAUTION]
> This driver might not work on other laptops produced by MSI. Use it at your own risk, we are not responsible for any
> damage suffered.

## Supported devices

Check [supported_devices.md](supported_devices.md) for supported devices and features

## Firmware naming

> [!IMPORTANT]
> This information is unofficial and is based on assumptions made during the analysis of the submitted devices.

[//]: # (|      EC      |     BIOS     |         Series          |)

[//]: # (|:------------:|:------------:|:-----------------------:|)

[//]: # (| xxxxbMSn.yzz | ExxxxbMS.yzz | Business/Creator/Gaming |)

[//]: # (| xxxxbWSn.yzz | ExxxxbWS.yzz | Workstation/CreatorPRO  |)

[//]: # (https://gist.github.com/luigiMinardi/4574708d404cdf4fe0da7ac6fe2314db)

|                                                      EC                                                       |                                             BIOS                                              |         Series          |
|:-------------------------------------------------------------------------------------------------------------:|:---------------------------------------------------------------------------------------------:|:-----------------------:|
| ${\color{red}xxxx}{\color{orange}b}{MS}{\color{yellow}n}{.}{\color{WildStrawberry}y}{\color{YellowOrange}zz}$ | ${E}{\color{red}xxxx}{\color{orange}b}{MS.}{\color{WildStrawberry}y}{\color{YellowOrange}zz}$ | Business/Creator/Gaming |
| ${\color{red}xxxx}{\color{orange}b}{WS}{\color{yellow}n}{.}{\color{WildStrawberry}y}{\color{YellowOrange}zz}$ | ${E}{\color{red}xxxx}{\color{orange}b}{WS.}{\color{WildStrawberry}y}{\color{YellowOrange}zz}$ | Workstation/CreatorPRO  |

+ ${\color{red}xxxx}$ - model code
+ ${\color{orange}b}$ - CPU/EC vendor
    + For BIOS:
        + A - AMD CPU
        + I - Intel CPU
    + For EC:
        + E - ENE
        + I - ITE
+ ${\color{yellow}n}$ - EC / board(?) model
+ ${\color{WildStrawberry}y}{\color{YellowOrange}zz}$ - version
    + ${\color{WildStrawberry}y}$ - board generation; firmware is incompatible if different
    + ${\color{YellowOrange}zz}$ - firmware version

## EC generations, WMI version

Now 2 EC layouts are known. They seem to be identical for all reported devices.

+ Generation 1 devices often have EC firmware available for download from the support page. Same firmware seems to be
  included to BIOS updates.
+ Generation 2 devices do not have EC firmware updates available for download on the support page.

