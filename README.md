libretro-manifest
=================

repo manifest for libretro project

This is an alternative method to libretro-fetch.sh for getting/keeping libretro project code up to date which may intigrate better with some devs' workflows. It allows for a local manifest file so that devs can re-map or add specific repositories in the libretro project with their personal working versions of that repository without imacting the mapping of any of the other community repositories. Read more about repo usage [here](http://source.android.com/source/using-repo.html). Read more about using a local manifest file [here](http://wiki.cyanogenmod.org/w/Doc%3a_Using_local_manifests#The_local_manifest).

### Setup and initial run
Instead of cloning libretro-super and then running libretro-fetch.sh do the following:

Make sure you have a bin/ directory in your home directory and that it is included in your path:
```bash
mkdir ~/bin
PATH=~/bin:$PATH
```
Download the Repo tool and ensure that it is executable:
```bash
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
```
Grab all the libretro projects:
```bash
mkdir ~/libretro-src
cd ~/libretro-src
repo init -u https://github.com/libretro/libretro-manifest.git
repo sync
repo forall -c git submodule update --init
```

### Subsequent runs (keep your local code up to date)
```bash
cd ~/libretro-src
repo sync
repo forall -c git submodule update
```
### Developer benefits
  Libretro is a big project. `repo` makes it easy to redirect one git repository (say for a single core) to your own personal developement git repo while staying up to date with the public repos of all the other projects. To do this you must override an entry in the default.xml file here in this repository. When you do `repo init -u https://github.com/libretro/libretro-manifest.git` the default.xml file from this repository gets pulled into ./.repo/  

Create a directory, ./.repo/local_manifests and then put a .xml file in it that overrides one of the repositories defined in default.xml  

For example if you have your own personal repository for libretro-super at https://github.com/l3iggs/libretro-super your local manifest.xml file might look like:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<manifest>
  <remote fetch="https://github.com/l3iggs/" name="mygithub"/>
  <project name="libretro-super" path="libretro-super" remote="mygithub" />
</manifest>
```
From then on when you do `repo sync` your own code will be pulled for libretro-super instead of the community's code.
