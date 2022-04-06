https://duckduckgo.com/?t=ffab&q=golang+download+file+from+url&ia=web

https://golangbyexample.com/download-image-file-url-golang/

https://gist.github.com/ego008/566376a774b0ad7a70d5521685b6da20

```golang
package main

import (
	"fmt"
	"io"
	"net/http"
	"os"
	"strings"
)

func downloadFromUrl(url string) {
	tokens := strings.Split(url, "/")
	fileName := tokens[len(tokens)-1]
	fmt.Println("Downloading", url, "to", fileName)

	// TODO: check file existence first with io.IsExist
	output, err := os.Create(fileName)
	if err != nil {
		fmt.Println("Error while creating", fileName, "-", err)
		return
	}
	defer output.Close()

	response, err := http.Get(url)
	if err != nil {
		fmt.Println("Error while downloading", url, "-", err)
		return
	}
	defer response.Body.Close()

	n, err := io.Copy(output, response.Body)
	if err != nil {
		fmt.Println("Error while downloading", url, "-", err)
		return
	}

	fmt.Println(n, "bytes downloaded.")
}

func main() {
	countries := []string{"GB", "FR", "ES", "DE", "CN", "CA", "ID", "US"}
	for i := 0; i < len(countries); i++ {
		url := "http://download.geonames.org/export/dump/" + countries[i] + ".zip"
		downloadFromUrl(url)
	}
}
```

```bash
download-tar $ gst
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	go.mod

nothing added to commit but untracked files present (use "git add" to track)
download-tar $ touch main.go
download-tar $ code .
download-tar $ go run main.go
Downloading http://download.geonames.org/export/dump/GB.zip to GB.zip
2321830 bytes downloaded.
Downloading http://download.geonames.org/export/dump/FR.zip to FR.zip
6984075 bytes downloaded.
Downloading http://download.geonames.org/export/dump/ES.zip to ES.zip
2977495 bytes downloaded.
Downloading http://download.geonames.org/export/dump/DE.zip to DE.zip
6578291 bytes downloaded.
Downloading http://download.geonames.org/export/dump/CN.zip to CN.zip
28278711 bytes downloaded.
Downloading http://download.geonames.org/export/dump/CA.zip to CA.zip
7841784 bytes downloaded.
Downloading http://download.geonames.org/export/dump/ID.zip to ID.zip
9599229 bytes downloaded.
Downloading http://download.geonames.org/export/dump/US.zip to US.zip
68917898 bytes downloaded.
download-tar $ ls
CA.zip		DE.zip		FR.zip		ID.zip		STORY.md	go.mod
CN.zip		ES.zip		GB.zip		README.md	US.zip		main.go
download-tar $
```
