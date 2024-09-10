# LineageOS 17.1

Before starting check [hardware requirement](https://source.android.com/setup/build/requirements).

### Installing dependencies and Repo
This is for informational purposes only, without any guarantee, you may need extra packages.

The following is from [CrDroid README](https://github.com/crdroidandroid/android).
The following is from [LineageOS-revived README](https://github.com/LineageOS-Revived/android/tree/lineage-17.1).
Some complements can be found [here](https://source.android.com/setup/build/initializing) too.

```bash
# Several packages are needed in order to build LineageOS & CrDroid
sudo apt install git-lfs bc bison build-essential ccache curl flex g++-multilib gcc-multilib git gnupg gperf imagemagick lib32ncurses5-dev lib32readline-dev lib32z1-dev liblz4-tool libncurses5 libncurses5-dev libsdl1.2-dev libssl-dev libwxgtk3.0-gtk3-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc zip zlib1g-dev

# Install Repo tool
# Make a directory where Repo will be stored and add it to the path
mkdir ~/bin
PATH=~/bin:$PATH

# Download Repo itself
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo

# Make Repo executable
chmod a+x ~/bin/repo
```

### How to build ###

```bash
# Create dirs
mkdir lineage17 ; cd lineage17

# Init repo
repo init -u https://github.com/LineageOS-Revived/android.git -b lineage-17.1 --git-lfs

# Init lfs
git-lfs install

# Clone my local repo
git clone https://github.com/clem-nesta/android_manifest_samsung_a3y17lte.git -b lineage-17.1 .repo/local_manifests

# Sync official Github repo
repo sync --no-repo-verify -c --force-sync --no-clone-bundle --no-tags --optimized-fetch --prune -j`nproc`

# Pull latest unofficial changes from Gerrit : https://review.lineageos.org/q/branch:lineage-17.1
  # Since only security patch up to february 2023 have been published on Github
  # you need to apply patchs from Gerrit to use latest security fixes
. build/envsetup.sh
  # Apply the patchs months by months in chronological order (here March 2023 for instance)
  # where Q_asb_2023-03 is the name of the topic dedicated to it on Gerrrit
repopick -t Q_asb_2023-03

# Build
. build/envsetup.sh && lunch lineage_a3y17lte-userdebug && mka clean && mka bacon -j`nproc`

```

## Credits
2019 @Astrako

2020 @Martin

2021-22 @debie @gonic

2023 @debie
