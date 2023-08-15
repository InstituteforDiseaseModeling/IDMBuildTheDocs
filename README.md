# BuildTheDocs
Build the pre-release readthedocs

## Name: Sphinx Azure Upload
**Description:** Builds HTML docs, and uploads to Azure Blob storage  
**Author:** Minerva Enriquez

## Inputs
- **Working Directory:**
  - **Description:** Documents root folder name
  - **Default:** 'docs'
  - **Required:** false

- **Blob Endpoint:**
  - **Description:** Azure blob endpoint
  - **Required:** true

- **Target Location:**
  - **Description:** Target Location and repo identifier, i.e. idm/repository
  - **Required:** true

- **Blob Container Name:**
  - **Description:** Azure container name
  - **Required:** true

- **Index HTML:**
  - **Description:** Landing page name for your documents project, it is by default index.html
  - **Default:** 'index.html'
  - **Required:** false

- **Make Text:**
  - **Description:** Determines if the sphinx build TAR file with the text file is generated. Valid values: **true** | **false**
  - **Default:** true

- **Tar File Name:**
  - **Description:** Unique TAR file name, please remember that the generation of a TAR file is fixed, and it overwrites the exitent file.
  - **Required:** true

- **Tar Target Folder:**
  - **Description:** Tar destination Folder Name
  - **Required:** false
  - **Default:** 'download/tar'

- **Service Principal Credentials:**
  - **Description:** BLOB_JSON_SERVICE_PRINCIPAL_CREDENTIALS
  - **Required:** true

- **Account Name:**
  - **Description:** Azure Account name
  - **Required:** true

- **CDN Profile Name:**
  - **Description:** Blob container CDN web profile name
  - **Required:** true

- **CDN Endpoint Name:**
  - **Description:** Blob cdn end point name
  - **Required:** true

- **Resource Group:**
  - **Description:** Resource group name
  - **Required:** true

- **PR Update Token:**
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
. . . 

    - name: Install emodpy-hiv requirements
      run: |
        >> Here your installation steps including the requiremets to build your docs (like Sphinx ) <<

    - name: Build Documentation for Preview and Tar file
      uses:  IDMPublicActions/BuildTheDocs@v1.0.2
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

    - name: ALL YOUR LINKS
      run: |
        echo "${{ env.LINK_URL }}"
        echo "${{ env.LINK_TO_TAR }}"

```
