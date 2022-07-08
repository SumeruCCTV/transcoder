# Transcoding library in Go
Fork of Floostack's transcoding library.

## Download from GitHub

```shell
go get github.com/SumeruCCTV/transcoder
```

## Example

```go
package main

import (
	"log"

	ffmpeg "github.com/SumeruCCTV/transcoder/ffmpeg"
)

func main() {

	hwaccel := "cuvid"
	videoCodec := "h264_cuvid"

	inputOpts := ffmpeg.Options{
		Hwaccel:    &hwaccel,
		VideoCodec: &videoCodec,
	}

	format := "mp4"
	overwrite := true

	outputOpts := ffmpeg.Options{
		OutputFormat: &format,
		Overwrite:    &overwrite,
	}

	ffmpegConf := &ffmpeg.Config{
		FfmpegBinPath:   "/usr/local/bin/ffmpeg",
		FfprobeBinPath:  "/usr/local/bin/ffprobe",
		ProgressEnabled: true,
	}

	progress, err := ffmpeg.
		New(ffmpegConf).
		Input("/tmp/avi").
		Output("/tmp/mp4").
		WithInputOptions(inputOpts).
		WithOutputOptions(outputOpts).
		Start()

	if err != nil {
		log.Fatal(err)
	}

	for msg := range progress {
		log.Printf("%+v", msg)
	}
}
```
