---
layout: default
title:  "top-secret"
date:   2022-04-13
---

### Top Secret

**description**: The internet post man gave me this but I don't get it.
He said it was top secret so the sender encrypted it with some RAW encryption and asked to not share it with anyone. I really trust you so can you decrypt this for me?

**attachment**: "TopSecret.zip" in [Google Drive](https://drive.google.com/drive/folders/1_f0PLreUcap2YW33Yfmi_Hj1cVyd9u6w?usp=sharing) with following files:

```
TopSecret
├── TopSecret.txt
└── key.private
```

##### Solution:

At beginning the decrypted file from `openssl` with the key file key.private was just gibberish. Then by re-reading the question carefully, I noticed there's a hugh hint about "RAW". By adding the `-raw` option in the command, I successfully decrypt the file.

```sh
openssl rsautl -decrypt -in TopSecret.txt -raw -out decrypt.txt -inkey key.private
```

