---
layout: post
title: Extracting tar files
---

This blog will explain you how to extract the tar file using go.

Tar files contain the header which we need to understand to extract the content of the tar file.
For much details follow the [blog](https://medium.com/learning-the-go-programming-language/working-with-compressed-tar-files-in-go-e6fe9ce4f51d) from @vladimirvivien.

Extracting a tar file is three step process :
1. Read the file as `io.Reader`.
2. Create `gzip.Reader` from `io.Reader`.
3. Create `tar.Reader` from `gzip.Reader`.
4. Use this new reader to process the zipped file iteratively.


<script src="https://gist.github.com/subh007/25b96d3cea03259d697153f25f24e3f2.js"></script>