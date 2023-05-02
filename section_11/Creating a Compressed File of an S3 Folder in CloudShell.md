# Creating a Compressed File of an S3 Folder in CloudShell


```shell
# The S3 URI for the Folder Containing the DeepRacer Model
S3_URI="s3://bucket-name/.../"

sudo -E /usr/local/bin/aws s3 cp "$S3_URI" "/s3/$(basename "$S3_URI")" --recursive
sudo zip -r "/s3/$(basename "$S3_URI").zip" "/s3/$(basename "$S3_URI")"
sudo -E /usr/local/bin/aws s3 cp "/s3/$(basename "$S3_URI").zip" "$(dirname "$S3_URI")/$(basename "$S3_URI").zip"
sudo rm -r /s3
```