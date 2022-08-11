# warp-upload-download-example
Example of Upload and Download in Rust using Warp

Run with `cargo run`. This starts a server at http://localhost:8080 with two routes:

* `POST /upload` - you can upload a file here
* `GET /files/*` - download the uploaded files

Example upload request:

```bash
curl --location --request POST 'http://localhost:8080/upload' \
--header 'Content-Type: multipart/form-data' \
--form 'file=@/home/somewhere/picture.png'
```

The file is saved using a random id as a name, with the extension. The name is logged, so you can see that it worked.

The max file-size is configured at ~5 MB.

# Uploader
```sh
#!/bin/sh
uuid=`ssh backup 'uuid=$(uuidgen) && ln -s ~/projects/warp-upload-download-example/files ~/projects/warp-upload-download-example/"$uuid".png && echo "$uuid"'`

#raspistill -o ./"$uuid".png
cp ./test.png ./"$uuid".png

curl --location --request POST 'http://api2.you-domain.net/upload' --header 'Content-Type: multipart/form-data' --form 'file=@./'"$uuid"'.png'

rm ./"$uuid".png

ssh backup 'unlink ~/projects/warp-upload-download-example/'"$uuid"'.png'
```
