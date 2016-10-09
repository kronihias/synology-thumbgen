# Thumbnail Generator for Synology PhotoStation
Creating thumbnails on the consumer level DiskStation NAS boxes by Synology is incredibly slow which makes indexing of large photo collections take forever to complete. It's much, much faster (days -> hours) to generate the thumbnails on your desktop computer over e.g. SMB using this small Python (v. 3.5) script.

## Usage
`python dsthumbgen.py --directory <path>`

Example: `python dsthumbgen.py --directory c:\photos`

Subdirectories will always be processed.
The script supports raw camera files.

## Requirements
The script needs Python 3.5 and some additional libraries:

Pillow, exifread, rawpy, numpy

```
# install python dependencies
pip install Pillow exifread rawpy numpy
```

Mount the synology photo drive using NFS to your Linux/OSX machine (NFS is required to support the folder name @eaDir)

```
mkdir /mnt/photo
sudo mount DS_IP:/volume1/photo /mnt/photo/
```
Now start creating thumbnails for all images:

```
python dsthumbgen.py --directory /mnt/photo/
```

## Good to know
Given a file and folder structure like below:

```
c:\photos\001.jpg
c:\photos\dir1\002.jpg
```

...the utility will create the following:

```
c:\photos\@eaDir\001.jpg\SYNOPHOTO_THUMB_XL.jpg
c:\photos\@eaDir\001.jpg\SYNOPHOTO_THUMB_B.jpg
c:\photos\@eaDir\001.jpg\SYNOPHOTO_THUMB_M.jpg
c:\photos\@eaDir\001.jpg\SYNOPHOTO_THUMB_PREVIEW.jpg
c:\photos\@eaDir\001.jpg\SYNOPHOTO_THUMB_S.jpg
c:\photos\dir1\@eaDir\002.jpg\SYNOPHOTO_THUMB_XL.jpg
c:\photos\dir1\@eaDir\002.jpg\SYNOPHOTO_THUMB_B.jpg
c:\photos\dir1\@eaDir\002.jpg\SYNOPHOTO_THUMB_M.jpg
c:\photos\dir1\@eaDir\002.jpg\SYNOPHOTO_THUMB_PREVIEW.jpg
c:\photos\dir1\@eaDir\002.jpg\SYNOPHOTO_THUMB_S.jpg
```


```
# remove any existing thumbnail directories, dry-run check print out before running next command!
find /volume1/photos -type d -name '@eaDir' -exec echo '{}' \;

# remove any existing thumbnail directories
find /volume1/photos -type d -name '@eaDir' -exec rm -rf '{}' \;

```
