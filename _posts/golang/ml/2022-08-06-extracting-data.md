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


``` golang
func extractFile(filename string) {
	gzipstream, err := os.Open(filename)
	if err != nil {
		log.Error("error while reading tar : %v", err)
		return
	}
	defer gzipstream.Close()

	uncompressedStream, err := gzip.NewReader(gzipstream)
	if err != nil {
		log.Error("ExtractTarGz: NewReader failed")
	}

	tarReader := tar.NewReader(uncompressedStream)

	for {
		header, err := tarReader.Next()
		if err == io.EOF {
			break
		}
		if err != nil {
			log.Error("ExtractTarGz: Next() failed: %s", err.Error())
		}

		switch header.Typeflag {
		case tar.TypeReg:
			f, err := os.Create(header.Name)
			if err != nil {
				log.Error("create failed : %v", err)
			}
			io.Copy(f, tarReader)
			f.Close()
		}
	}
}
```