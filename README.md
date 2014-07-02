# Modified crontab

Reads crontabs in /home/users/crontabs/ and writes them there.

This is needed for the Magento Monitor Cron editor.


## Building the Debian package

```
```
# 1. Make sure you have a git-buildpackage environment ready. For this, see the wiki
#    (tl;dr: `sudo apt-get install git-buildpackage; sudo DIST=squeeze ARCH=i386 git-pbuilder create`)
# 2. Create the debian/changelog:
VERSION=$(date "+%Y%m%d.%H%M.1")
git-dch --debian-tag="%(version)s" --new-version=$VERSION --debian-branch byte --release

# 3. Commit the debian/changelog:
git add debian/changelog
git commit -m "Annotate changelog"

# 4. Add the Debian remote
git remote add debian git://git.debian.org/git/pkg-cron/pkg-cron.git

# 5. Build
sudo git-buildpackage --git-pbuilder --git-dist=squeeze --git-arch=i386 --git-debian-branch=byte

# 6. Test
# Test if the build succeeded

# 7. Tag the current version, take new version from changelog
git tag $VERSION
git push
git push --tags

# 8. Push
# Make sure you have the dput.cf install that comes with your 
# dev machine. This might take some work to get right.
dput -uf squeeze-staging $changesfile  # To staging
dput -uf squeeze $changesfile  # To production
```
