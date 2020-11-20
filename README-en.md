# Based on tinymce attachment upload plugin

### Plug-in features

1. Simple, not so fancy UI
2. Support upload progress
3. Support drag and drop upload, does not affect the drag and drop of the original image plug-in

![img](https://raw.githubusercontent.com/NebulaStudio/tinymce_attachment_plugin/master/attachment.gif)

### how to use

To use this plugin, you need to pay attention to 4 parameter configurations

#### 1. attachment_max_size: Optional parameters, the maximum limit for uploading a single file, the unit is byte, the default is 200 M

Example: Limit upload size to 100M

```
tinymce.init({
    attachment_max_size: 100 * 1024 * 1024
});
```

#### 2. attachment_assets_path: The path of static resources, used for file icon and upload waiting animation, upload error information prompt icon. PS: Static resources are located under /assets/icons of the project.

Example:



```
tinymce.init({
    attachment_assets_path: 'assets/icons/'
});
```

#### 3. attachment_upload_handler: Upload callback function.

| Parameter name   | Type     | Description |
| ---------------- | -------- | ----------------------- ------------------- |
| file             | object   | Uploaded file object |
| successCallback  | Function | The callback function for successful upload, the parameter is the file download address. |
| failureCallback  | Function | The callback function for upload failure, the parameter is the error message. |
| progressCallback | Function | Upload progress callback function, the parameter is the percentage of progress. |



Take axios as an example:

```
const axios = require('axios');
tinymce.init({
    attachment_upload_handler: upload(file, successCallback, failureCallback, progressCallback) {
        axios.post('/upload_hander', {
            data: file,
            onUploadProgress: function(e) {
                const progress = (e.loaded / e.total * 100 | 0) + '%';
                progressCallback(progress);
            }
        }).then((response) => {
            successCallback(response.data.url);
        }).catch((error) => {
            failureCallback(`上传失败:${error.message}`)
        });
    }
});
```

#### 4. Add css code in the style file corresponding to the content_css of initial tinymce.

Example: the following configuration file

```
tinymce.init({
    content_css: 'tinymce/skins/content/snow/content.css',
});
```

You need to add the following css to the tinymce/skins/content/snow/content.css file:

```
.attachment {
    cursor: pointer !important;
}
.upload_error {
    background: #FFE5E0;
    border: 1px solid #EA644A;
}
.attachment > img {
    width: 16px;
    vertical-align: middle;
    padding-right:4px;
}
.attachment > a {
    text-decoration: none;
    vertical-align: middle;
}

.attachment > span {
    vertical-align: middle;
    padding-right:4px;
}
```

### Known bug

After selecting the file to upload, a piece of uneditable text will be inserted into the editing area.

```
contenteditable = 'false'
```

Pressing Backspace at this time may not delete or the cursor jumps to the first line.
This problem is a bug of the editor and has nothing to do with this plugin.


https://github.com/tinymce/tinymce/issues/3559

https://github.com/tinymce/tinymce/issues/5039

https://github.com/tinymce/tinymce/issues/5679

# License

MIT
