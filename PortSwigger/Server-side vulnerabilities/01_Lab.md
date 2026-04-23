## Introduction
01_Lab: File path traversal, simple case

This lab demonstrates a **path traversal** vulnerability in the product image display functionality, allowing arbitrary file access on the server. The exploitation is straightforward and consists of manipulating the image-loading parameter until it reaches the `/etc/passwd` file.

## Exploitation

By analyzing the site requests in Burp Suite, it is possible to identify an endpoint responsible for loading product images. The original request uses a `filename` parameter with what appears to be a legitimate image name.

`GET /image?filename=71.jpg`

![Intercept](img/Lab1_1.png)

With that in mind, the parameter was modified to attempt directory escape and reach a sensitive system file.

`GET /image?filename=../../../etc/passwd`

This change caused the server to return the contents of `/etc/passwd`, confirming the path traversal flaw and validating unauthorized access to the file.

![ChangeParams](img/Lab1_2.png)

After the parameter was changed, the application no longer rendered the image correctly, since the response contained text instead of a valid image file.

![ImgNoContent](img/Lab1_3.png)
