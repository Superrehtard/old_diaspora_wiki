# Bash Scripts for Admins

This Page contains quick & dirty scripts you could need if you administrate a Diaspora Pod

##  Create Missing Thumbnails

It can occur that not all thumbnails are generated, this script checks it and recreates them

```
PHOTONAME=`mysql -Bse "SELECT remote_photo_name FROM diaspora.posts WHERE type='Photo'" 
for PHOTO in $PHOTONAME
do
test -e ./diaspora/public/uploads/images/$PHOTO || echo $PHOTO does not exist
test -e ./diaspora/public/uploads/images/thumb_small_$PHOTO || cp ./diaspora/public/uploads/images/$PHOTO ./diaspora/public/uploads/images/thumb_small_$PHOTO && mogrify -resize 50x50
test -e ./diaspora/public/uploads/images/thumb_medium_$PHOTO || cp ./diaspora/public/uploads/images/$PHOTO ./diaspora/public/uploads/images/thumb_medium_$PHOTO && mogrify -resize 100x100
test -e ./diaspora/public/uploads/images/thumb_large_$PHOTO || cp ./diaspora/public/uploads/images/$PHOTO ./diaspora/public/uploads/images/thumb_large_$PHOTO && mogrify -resize 300x300
test -e ./diaspora/public/uploads/images/scaled_full_$PHOTO || cp ./diaspora/public/uploads/images/$PHOTO ./diaspora/public/uploads/images/scaled_full_$PHOTO && mogrify -resize 700x700
done
```