# gecko-workflow-instruction
An instruction of how to develope Gecko/Firefox in git

## Prerequisites

### Linux
*   ```wget -q https://hg.mozilla.org/mozilla-central/raw-file/default/python/mozboot/bin/bootstrap.py -O bootstrap.py && python bootstrap.py```

### MacOSX
*   ```curl https://hg.mozilla.org/mozilla-central/raw-file/default/python/mozboot/bin/bootstrap.py > bootstrap.py && python bootstrap.py```

### Windows
*   Install [git for Windows](https://git-scm.com/download/win) and select "**Check as-is, commit as-is**" in **Configuration the line ending conversions** while installation.
*   Install [Visual Studio Community 2015](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx#d-community)
    *   If VS installation gets stuck on **KB2999226**, use VS **ISO** instead
Download and install the MozillaBuild package
*   For [more detail](https://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide/Build_Instructions/Windows_Prerequisites)...

## git-cinnabar
*   Install Mercurial 
    *   ```pip install mercurial```
*   Install [git-cinnabar](https://github.com/glandium/git-cinnabar)
    *  ```git clone https://github.com/glandium/git-cinnabar.git```
    *  ```git submodule update --init```
    *   Configure PATH

## Workflow for [Gecko development](https://github.com/glandium/git-cinnabar/wiki/Mozilla:-A-git-workflow-for-Gecko-development)
*   Init
```
git init gecko
cd gecko
git config fetch.prune true
git config push.default upstream
```
*   Configure Inbound/Central
```
git remote add central hg::https://hg.mozilla.org/mozilla-central -t branches/default/tip
git remote add inbound hg::https://hg.mozilla.org/integration/mozilla-inbound -t branches/default/tip
git remote set-url --push inbound hg::ssh://hg.mozilla.org/integration/mozilla-inbound
git config remote.central.fetch +refs/heads/branches/*/tip:refs/remotes/central/*
git config remote.inbound.fetch +refs/heads/branches/*/tip:refs/remotes/inbound/*
git config remote.central.fetch +refs/heads/branches/default/tip:refs/remotes/central/default
git config remote.inbound.fetch +refs/heads/branches/default/tip:refs/remotes/inbound/default
```
*   Configure Try
```
git remote add try hg::https://hg.mozilla.org/try
git config remote.try.skipDefaultUpdate true
git remote set-url --push try hg::ssh://hg.mozilla.org/try
git config remote.try.push +HEAD:refs/heads/branches/default/tip
```
*   Configure fx-team
```
git remote add fx-team hg::https://hg.mozilla.org/integration/fx-team -t branches/default/tip
git config remote.fx-team.fetch +refs/heads/branches/*/tip:refs/remotes/fx-team/*
git config remote.fx-team.fetch +refs/heads/branches/default/tip:refs/remotes/fx-team/default
```
*   Fetch updates for all remote branches
    *   ```git remote update```
*   Note
    *   [gecko-dev](http://github.com/mozilla/gecko-dev) and git-cinnabar repo have different git-hash
    *   [Tree Rules/Integration](https://wiki.mozilla.org/Tree_Rules/Integration)
    *   If you see **locale.Error: unsupported locale setting** in Ubuntu, please do these:
```
export LC_ALL="en_US.UTF-8"
export LC_CTYPE="en_US.UTF-8"
sudo dpkg-reconfigure locales
```
## Work with github (optional)
*   If you need a place to share your patches between different work machines...
*   Github is an option
    *   Fork [fx-dev-playground/gecko](https://github.com/fx-dev-playground/gecko)
    *   Configure your github remote
```
git remote add github https://github.com/<user>/gecko.git
git checkout central/default
git checkout -b default
git push -u github default
```

## mach (German for do)
*   Build
    *   ```./mach build [-jn]```
*   Clean
    *   ```./mach clobber```
*   Run
    *   ```./mach run```
*   mochitest
    *   ```./mach mochitest [path-to-test]```
*   [mochilog](https://gist.github.com/weilonge/dff7932227ab029fb23463ca18b74a64) - Colorized mochitest log
    *   ```./mach mochitest | mochilog```
*   Enable tab completion
    *   ```source python/mach/bash-completion.sh```
*   For [more detail](https://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide/mach)...

## try server
*   Get Level 1 Access first!!! [#](https://www.mozilla.org/en-US/about/governance/policies/commit/)
    *   Prepare SSH public key [#](https://help.github.com/articles/generating-ssh-keys/)
    *   [Fire a bug!](https://bugzilla.mozilla.org/enter_bug.cgi?product=mozilla.org&component=Repository%20Account%20Requests) [examples](https://bugzilla.mozilla.org/buglist.cgi?f35=status_whiteboard&o13=substring&list_id=12953991&f44=short_desc&v26=%28Level&f39=OP&o33=substring&f30=CP&o14=substring&o45=substring&o16=substring&f13=component&o2=substring&f23=product&f29=CP&f25=alias&v24=%28Level&v42=for&o4=substring&f27=status_whiteboard&o12=substring&v36=1%29&o17=substring&f21=OP&v2=Commit&v41=for&f32=OP&f10=OP&f19=CP&o35=substring&v43=for&o26=substring&o3=substring&v6=Commit&v45=for&v34=1%29&f34=short_desc&o24=substring&v3=Commit&f42=component&f15=alias&o28=substring&f46=cf_crash_signature&o41=substring&o6=substring&f4=alias&f2=product&v12=Access&bug_status=RESOLVED&bug_status=VERIFIED&bug_status=CLOSED&o43=substring&f38=CP&f17=status_whiteboard&v14=Access&f7=cf_crash_signature&o5=substring&f36=cf_crash_signature&j32=OR&v27=%28Level&f26=short_desc&v25=%28Level&j22=OR&f28=cf_crash_signature&o34=substring&j11=OR&v5=Commit&o15=substring&f12=product&o18=substring&v18=Access&o46=substring&f14=keywords&f43=alias&f24=component&v13=Access&f47=CP&v16=Access&v4=Commit&v15=Access&f22=OP&f48=CP&f1=OP&f31=OP&f37=CP&o7=substring&j40=OR&f20=CP&v35=1%29&f8=CP&f0=OP&o25=substring&f33=alias&v28=%28Level&f18=cf_crash_signature&v33=1%29&v44=for&o42=substring&f9=CP&v7=Commit&f45=status_whiteboard&o23=substring&query_format=advanced&v23=%28Level&j1=OR&f3=component&v17=Access&f11=OP&f41=product&o36=substring&f5=short_desc&f6=status_whiteboard&v46=for&o44=substring&o27=substring&f16=short_desc&f40=OP)
    *   Get your voucher!
*   [trychooser](http://trychooser.pub.build.mozilla.org/) - helper to generate try-syntax
*   3 ways to push to try server…
    *   ```./mach try <TRYCHOOSER_SYNTAX>```
        *   Recommanded!!!
    *   Use git command to push to try server directly
        *   ```git commit --allow-empty -m "try: <TRYCHOOSER_SYNTAX>"```
        *   ```git push try```
    *   ```git push-to-try```
        *   [detail](https://github.com/mozilla/moz-git-tools#git-push-to-try)
*   Get a mail from Try server
    *   Subject: try submission 464634fa0673: try: ...
    *   Links: treeherder + logs

## mozreview
*   [Review Board](https://reviewboard.mozilla.org/)
*   Configuring [mozreview](http://mozilla-version-control-tools.readthedocs.org/en/latest/mozreview/install.html) + [git](http://mozilla-version-control-tools.readthedocs.org/en/latest/mozreview/install-git.html#mozreview-install-git)
*   [SSH Configuration](http://mozilla-version-control-tools.readthedocs.org/en/latest/hgmozilla/auth.html#auth-ssh)
*   [MozReview Doc](http://mozilla-version-control-tools.readthedocs.org/en/latest/mozreview.html)
*   mozreview - ssh configuration
    *   Add these lines into **~/.ssh/config** and don’t forget to change **me**
```
Host hg.mozilla.org
  User me@mozilla.com

Host reviewboard-hg.mozilla.org
  User me@mozilla.com
```
*   Verify your account
    *   ```ssh hg.mozilla.org```
    *   ```ssh reviewboard-hg.mozilla.org```
*   Prepare [Bugzilla API Key](https://bugzilla.mozilla.org/userprefs.cgi?tab=apikey)
*   Manually Associating Your LDAP Account
    *   ```ssh reviewboard-hg.mozilla.org mozreview-ldap-associate```
    *   Enter your Bugzilla e-mail address
    *   Enter a Bugzilla API Key
*   Installing MozReview Git Tools
    *   ```./mach mercurial-setup```
*   ```git config --global```
    *   ```bz.username``` - Bugzilla Username
    *   ```bz.apikey``` - Bugzilla [API Key](https://bugzilla.mozilla.org/userprefs.cgi?tab=apikey)
    *   ```mozreview.nickname```
*   ```git mozreview configure```
*   mozreview - usage
    *   Commit Format - ```Bug XXXX - Commit description.; r?reviewer```
    *   ```git mozreview push```
    *   [Bug 1259355](https://bugzilla.mozilla.org/show_bug.cgi?id=1259355) for practicing mozreview

## Interacting with Bugzilla - git-bz-moz
*  Install [git-bz-moz](https://github.com/mozilla/git-bz-moz/) ([man page](https://github.com/mozilla/git-bz-moz/blob/master/git-bz.txt))
*  Import a patch from a bug to review
    *   ```git bz apply <BUG_ID>```
*  Post a patch to a bug for feedback
    *   ```git bz attach -ne HEAD```
*  Prefer MozReview for review requests!

## Attach a HG patch manually 
*   Use [moz-git-tools](https://github.com/mozilla/moz-git-tools) to generate hg patch
    *   ```git format-patch HEAD~```
    *   ```git patch-to-hg-patch <git-patch-path>```
*   However, some reviewers think git patch is acceptable.

## [eslint](https://wiki.mozilla.org/DevTools/CodingStandards)
*   Installing ESLint and Recommended plugins
    *   ```./mach eslint --setup```
*   Running ESLint from the command line
    *   ```./mach eslint path/to/directory```

## [Artifact builds](https://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide/Build_Instructions/Artifact_builds)
*   Release build - .mozconfig
```
# Automatically download and use compiled C++ components:
ac_add_options --enable-artifact-builds

# Write build artifacts to:
mk_add_options MOZ_OBJDIR=./objdir-frontend
```
*   Debug build - .mozconfig
```
# Enable debug version of the pre-build binary artifact
export MOZ_DEBUG="1"

# Automatically download and use compiled C++ components:
ac_add_options --enable-artifact-builds

# Write build artifacts to:
mk_add_options MOZ_OBJDIR=./objdir-frontend-debug-artifact
```

## [icecc](https://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide/Using_Icecream)

### MacOSX

*   Install Prerequisites
    *   Please select a proper [scheduler IP address](https://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide/Using_Icecream#Running_a_scheduler).
```
xcode-select --install
brew install lzo
git clone https://github.com/mystor/icecc-osx-moztor ~/icecream
sudo ~/icecream/install.sh SCHEDULER_IP_ADDRESS
```
*   Add these two lines in .mozconfig
```
CC="$HOME/icecream/cc"
CXX="$HOME/icecream/c++"
```
*   Use -j60 to see the significant throughput
    *   ```./mach build -j60```

### Ubuntu/Debian Linux
*   ```sudo apt-get install icecc```
*   Add these two lines in .mozconfig
```
ac_add_options --with-compiler-wrapper=icecc
ac_add_options --disable-ccache
```
*   Use -j60 to see the significant throughput
    *   ```./mach build -j60```
