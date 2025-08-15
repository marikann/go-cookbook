---
title: 'File Archiving with tar and zip'
description: 'Create and extract archives using Go archive/tar and archive/zip packages'
date: '2025-08-15'
category: 'Files'
---

Go provides built-in support for creating and extracting common archive formats through the `archive/tar` and `archive/zip` packages. These are useful for file distribution, backups, and data packaging.

## Working with TAR Archives

Create and extract TAR archives for Unix-style file archiving:

```go
package main

import (
	"archive/tar"
	"fmt"
	"io"
	"os"
	"path/filepath"
)

func createTar(sourceFiles []string, outputPath string) error {
	outFile, err := os.Create(outputPath)
	if err != nil {
		return err
	}
	defer outFile.Close()

	tarWriter := tar.NewWriter(outFile)
	defer tarWriter.Close()

	for _, srcFile := range sourceFiles {
		if err := addToTar(tarWriter, srcFile); err != nil {
			return err
		}
	}
	return nil
}

func addToTar(tw *tar.Writer, filePath string) error {
	file, err := os.Open(filePath)
	if err != nil {
		return err
	}
	defer file.Close()

	stat, err := file.Stat()
	if err != nil {
		return err
	}

	hdr := &tar.Header{
		Name: filepath.Base(filePath),
		Mode: 0644,
		Size: stat.Size(),
	}

	if err := tw.WriteHeader(hdr); err != nil {
		return err
	}
	_, err = io.Copy(tw, file)
	return err
}

func extractTar(tarPath, destDir string) error {
	file, err := os.Open(tarPath)
	if err != nil {
		return err
	}
	defer file.Close()

	tarReader := tar.NewReader(file)
	for {
		hdr, err := tarReader.Next()
		if err == io.EOF {
			break
		}
		if err != nil {
			return err
		}

		outPath := filepath.Join(destDir, hdr.Name)
		os.MkdirAll(filepath.Dir(outPath), 0755)

		outFile, err := os.Create(outPath)
		if err != nil {
			return err
		}
		io.Copy(outFile, tarReader)
		outFile.Close()
	}
	return nil
}

func main() {
	// Create sample files.
	testFiles := []string{"notes.txt", "config.txt"}
	for i, name := range testFiles {
		os.WriteFile(name, []byte(fmt.Sprintf("Sample data %d", i+1)), 0644)
	}

	// Create and extract TAR.
	createTar(testFiles, "backup.tar")
	extractTar("backup.tar", "restored/")
	fmt.Println("TAR operations completed")
}
```

## Working with ZIP Archives

Create and extract ZIP archives for cross-platform compatibility:

```go
package main

import (
	"archive/zip"
	"fmt"
	"io"
	"os"
	"path/filepath"
)

func packageZip(fileList []string, zipName string) error {
	zipFile, err := os.Create(zipName)
	if err != nil {
		return err
	}
	defer zipFile.Close()

	writer := zip.NewWriter(zipFile)
	defer writer.Close()

	for _, fileName := range fileList {
		if err := writeToZip(writer, fileName); err != nil {
			return err
		}
	}
	return nil
}

func writeToZip(zw *zip.Writer, srcPath string) error {
	srcFile, err := os.Open(srcPath)
	if err != nil {
		return err
	}
	defer srcFile.Close()

	// Create compressed entry.
	zipEntry, err := zw.Create(filepath.Base(srcPath))
	if err != nil {
		return err
	}

	_, err = io.Copy(zipEntry, srcFile)
	return err
}

func unpackZip(zipPath, targetDir string) error {
	zipReader, err := zip.OpenReader(zipPath)
	if err != nil {
		return err
	}
	defer zipReader.Close()

	os.MkdirAll(targetDir, 0755)

	for _, entry := range zipReader.File {
		entryReader, err := entry.Open()
		if err != nil {
			return err
		}

		outputPath := filepath.Join(targetDir, entry.Name)
		outputFile, err := os.Create(outputPath)
		if err != nil {
			entryReader.Close()
			return err
		}

		io.Copy(outputFile, entryReader)
		outputFile.Close()
		entryReader.Close()
	}
	return nil
}

func main() {
	// Generate test content.
	documents := []string{"report.txt", "summary.txt"}
	for i, doc := range documents {
		content := fmt.Sprintf("Report section %d\nAnalysis results here", i+1)
		os.WriteFile(doc, []byte(content), 0644)
	}

	// Package and unpack.
	packageZip(documents, "reports.zip")
	unpackZip("reports.zip", "unpacked/")
	fmt.Println("ZIP packaging completed")
}
```

## Best Practices

- Use TAR for Unix/Linux environments and ZIP for cross-platform compatibility.
- Validate file paths during extraction to prevent directory traversal attacks.
- Use compression (zip.Deflate) for better space efficiency in ZIP archives.

## Common Pitfalls

- Not handling directory creation during extraction, leading to failed file creation.
- Forgetting to close archive writers, which may result in incomplete archives.
- Not sanitizing file paths during extraction, creating security vulnerabilities.

## Performance Tips

- Use buffered I/O for large files to improve archive creation and extraction speed.
- Consider parallel processing for multiple independent files in large archives.
- Choose appropriate compression levels based on the trade-off between speed and size.
- Pre-allocate buffers for I/O operations when processing many files.
