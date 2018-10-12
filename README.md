# genup
Tool to update the **Portage**(5) tree, all installed packages, and kernel, under Gentoo Linux.

## Description
**genup** is  a  utility  intended  to  simplify the process of keeping your Gentoo system up to date.  When invoked, it automatically performs the following steps, in order:
* updates Portage tree (and active overlays), and syncs **eix**(1)
(using `emaint sync` / `eix-sync`)
* removes any prior **emerge**(1) resume history
(using `emaint --fix cleanresume`)
* ensures **Portage**(5) itself is up-to-date
(using `emerge --oneshot --update portage`)
* ensures **genup** itself is up-to-date (restarting if not)
(using `emerge --oneshot --update genup`)
* updates all packages in the @world set
(using `emerge --deep --with-bdeps=y --changed-use --update @world`)
* removes unreferenced packages
(using `emerge --depclean`)
* rebuilds any external modules (such as those for VirtualBox)
(using `emerge @module-rebuild --exclude '*-bin'`)
* rebuilds any packages depending on stale libraries
(using `emerge @preserved-rebuild`)
* updates any old perl(1) modules
(using `perl-cleaner --all`)
* resolves clashing config file changes (in interactive mode)
(using `dispatch-conf`)
* upgrades the kernel if possible (to staging, in _/boot_)
(using `buildkernel --stage-only`)
* removes unreferenced packages (again)
(using `emerge --depclean`)
* fixes missing shared library dependencies
(using `revdep-rebuild`)
* rebuilds any packages depending on stale libraries (again)
(using `emerge @preserved-rebuild`)
* removes any unused source tarballs (if desired)
(using `eclean --deep distfiles`)
* deploys new kernel from staging (if desired and available)
(using `buildkernel --copy-from-staging`)
* updates environment settings (as a precautionary measure)
(using `env-update`)
* runs any custom updaters in /etc/genup/updaters.d

The genup utility can be invoked in non-interative (default) or interactive mode (see the  **--ask**  option in the manpage).   Non-interactive  mode  is  suitable  for use in a scripted invocation, for example as part of a nightly **cron**(8) job.

## Installation

**genup** is best installed (on Gentoo) via its ebuild, available as part of the **sakaki-tools** [overlay](https://github.com/sakaki-/sakaki-tools).
Full instructions are provided as part of the [**Sakaki's EFI Install Guide**](https://wiki.gentoo.org/wiki/Sakaki's_EFI_Install_Guide) tutorial, on the Gentoo wiki.
