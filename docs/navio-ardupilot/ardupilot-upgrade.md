#### Overview

We're now in process of transitioning our deploy scheme. That's why we strongly encourage you to look into this article.
For a year our users had been asked to install a *deb* package with a ```dpkg -i``` command. This is a rather cumbersome way to get things done.
From now on we deploy the packages to our repository making to possible to use ```apt-get``` out of the box. The ugliest part of the former is two
*deb* packages existing in the wild with different upgrade steps required from users. That's what this entry is for.

#### General warnings

Please, backup your parameters before proceeding!

#### Upgrade

```bash
pi@navio: ~ $ sudo apt-get update && sudo apt-get dist-upgrade
```
![compass-warning](img/navio2-compass-recalibration.png)

Read the warning once more and please obey the instructions! The packages do have different compasses' configuration due to various reasons.

#### After upgrade

You won't need to set compass #2 as external anymore and apply some custom rotation. It is done automatically for you.
Unset compass #2 external option in GCS of choice.

#### Final thoughts

If this tutorial is confusing in any way, please let us now! We'd be glad to help you out and fix the instructions accordingly.
