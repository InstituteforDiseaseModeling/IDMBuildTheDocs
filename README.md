# BuildTheDocs
## Goal: 
This GitHub Action creates intermediate HTML and Text builds for Read the Docs and then uploads the outcomes to Azure Blob storage, providing the resulting links at the end. These links are permanent and won't expire. HTML builds get uploaded to a unique location, operating as fully functional ReadTheDocs instances. On the other hand, Text builds are compressed and distributed as downloadable tar.gz files.

**Output sample:** <br> 
- _https://idmdocsstaging.z5.web.core.windows.net/idm/InstituteforDiseaseModeling/emodpy-hiv/August15-162039/docs/index.html_
- _https://idmdocsstaging.z5.web.core.windows.net/idm/InstituteforDiseaseModeling/emodpy-hiv/download/tar/emodpy-hiv.tar.gz_

**Author:** Minerva Enriquez

## Table of contents
- [Inputs](#inputs)
- [Outputs](#output)
- [Sample](#sample)

## Inputs
- **working_directory:**
  - **Description:** Documents root folder name
  - **Default:** 'docs'
  - **Required:** false

- **blob_endpoint:**
  - **Description:** Azure blob endpoint
  - **Required:** true

- **target_location:**
  - **Description:** Target Location and repo identifier, i.e. idm/repository
  - **Required:** true

- **blob_container_name:**
  - **Description:** Azure container name
  - **Required:** true

- **index_html:**
  - **Description:** Landing page name for your documents project, by default it is index.html
  - **Default:** 'index.html'
  - **Required:** false

- **make_text:**
  - **Description:** Specifies if the a tar.gz file with the Text Build should be generated. Valid values are: **true** | **false**
  - **Default:** true

- **tar_file_name:**
  - **Description:** Unique TAR file name prefix, please note that the generated TAR file is placed under a fixed location and it will overwrite the existent file with the same name.
  - **Required:** true

- **tar_target_folder:**
  - **Description:** Tar destination Folder Name
  - **Required:** false
  - **Default:** 'download/tar'

- **service_principal_credentials:**
  - **Description:** BLOB_JSON_SERVICE_PRINCIPAL_CREDENTIALS
  - **Required:** true

- **account_name:**
  - **Description:** Azure Account name
  - **Required:** true

- **cdn_profile_name:**
  - **Description:** Blob container CDN web profile name
  - **Required:** true

- **cdn_endpoint_name:**
  - **Description:** Blob cdn end point name
  - **Required:** true

- **resource_group:**
  - **Description:** Resource group name
  - **Required:** true

- **pr_update_token:**
  - **Description:** Github token
  - **Required:** true
 
## Output
-  env.LINK_URL
-  env.LINK_TO_TAR


## Sample
Worflow sample:
[Implementation example on emodpy-hiv](https://github.com/InstituteforDiseaseModeling/emodpy-hiv/blob/8255d344620976af2dfb7d7184569a1da912d795/.github/workflows/emodpy-hiv-docs-rebuild.yml#L41)
_</br>Please note that example above is for internal access only_

```
     . . .  steps before - installing pre-requisites . . .

         

    - name: Build Documentation for HTML ReadTheDocs Preview and text build Tar file
      uses:  InstituteforDiseaseModeling/IDMBuildTheDocs@v1.0.0
      with:
         working_directory: 'docs'
         blob_endpoint: 'https://something.z5.web.core.windows.net'
         target_location: 'idm/${{ github.repository }}'
         blob_container_name: '$web'
         index_html: 'index.html'
         tar_file_name: 'AnyValidFileName'
         service_principal_credentials:  ${{ secrets.BLOB_JSON_SERVICE_PRINCIPAL_CREDENTIALS }} 
         account_name: 'idmdocsstaging'
         cdn_profile_name: 'abcdefg-cdn-webprofile'
         cdn_endpoint_name: 'abcdefg-cdn-webendpoint'
         resource_group: 'MyResourceGroupName'
         pr_update_token: ${{ secrets.GITHUB_TOKEN }}




    . . . steps after - for example, displaying the generated links . . .
    - name: ALL YOUR LINKS
      run: |
        echo "${{ env.LINK_URL }}"
        echo "${{ env.LINK_TO_TAR }}"

```
