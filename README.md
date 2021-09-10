# esp-idf-action

GitHub action to build ESP-IDF project using esp-idf framework. This action downloads the required ESP-IDF from espressif server and from github for latest branch

The `esp_idf_version` as follows
- `latest` (master branch)
- [ESP-IDF version list](https://github.com/espressif/esp-idf/tags)

**Note:**
The action runs on ubuntu latest and python3 as default interpreter.

### Example

```yml
# This is a esp idf workflow to build ESP32 based project

name: Build and Artifact the ESP-IDF Project

# Controls when the action will run. 
on:
  # Triggers the workflow on tag create like v1.0, v2.0.0 and so on
  push:
    tags:
     - 'v*'

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      - name: Checkout the code
        uses: actions/checkout@v2

      - name: Install ESP-IDF tools
        run: rmshub/esp-idf-action@v1
        with: 
            esp_idf_version: 'v4.3'

      - name: Build the repository
        run: |
          source ~/esp/esp-idf/export.sh
          idf.py set-target esp32
          idf.py build
      
      - name: Archive build output artifacts
        uses: actions/upload-artifact@v2
        with:
          name: build
          path: |
            build/bootloader/bootloader.bin
            build/partition_table/partition-table.bin
            build/${{ github.event.repository.name }}.bin
```

## Test

Currently this action verified with esp-idf v4.3

## License

This repository is licensed with the [MIT License](LICENSE).