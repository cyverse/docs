[:material-lock: Discovery Environment Admin Panel](https://de.cyverse.org/admin/)

## Administrative Panel

### Apps

Add/Edit/Delete applicatryouions

Requires a pre-existing Tool (Container Template)

### DOI Requests SOP

!!! Note "Prerequisites and information"

    - To view DOI Requests, admin priviledges are required. 

    - For a DOI request to be submitted, users should follow the stelps outlined in the [quick start tutorial](https://learning.cyverse.org/ds/doi/).

    - DOI Requests are visible in the [**Admin** sidebar menu](https://de.cyverse.org/admin/doi) of the DE. 

#### Approaching the DOI

1. A new Intercom ticket will automatically be created when a new DOI has been requested via the DE.
2. Open 'DOI requests' in the admin tab of the DE.
3. Select the current request, and click on Update Request. Change status from Submitted to Pending, if you don't plan to work on it right away.
4. When you are ready review, change status to Evaluation.
5. Check the user's subscription tier:
    - If they only have a *Basic* subscription plan they are not entitled to any DOIs. In that case change the DOI status to *Rejected*. Include a comment as to why it was rejected. 
    - If the user has a paid plan make sure they haven't already received all the DOIs they are entitled to for the year. Very few users request more than one DOI per year. You can see all of the requests (with dates) in the DOI request area of the DE admin page.

#### Evaluating the request in the DE

1. Review the dataset in `/iplant/home/shared/commons_repo/staging` for:
    1. A `ReadMe` file that explains the dataset and includes a manifest/inventory
        - Manifest/inventory described [here](https://learning.cyverse.org/ds/doi/)
    2. Organized in a sensible fashion
    3. If the project includes sequences, there should be information about the specimens, **with a list of BioSample IDs**
    4. [CyVerse Curated Data Folder-Naming Guidelines](https://learning.cyverse.org/ds/doi/#12-name-your-top-level-folder-according-to-the-data-commons-naming-conventions)
        - These are flexible. If someone has a good reason to use a different name, it is allowed, but it should be roughly similar to the guidelines
    5. No spaces in folder or file names.
2. If changes are needed, email the requestor and ask them to make the changes. Be sure to provide the location of the data (`/iplant/home/commons_repo/staging/$folder_name`). 
    1. Makes sure the requestor has at least write permission. If you have asked them to delete anything, give them own permission.
    2. It is useful to record what changes are requested in the comments in the status field in the request (in the Admin tab of the DE).
3. In the DE, view the metadata (check the box next to the folder, click 'more actions', 'metadata') and apply the **DataCite 4.1 template** (using the 'more actions', 'view in template'), then review the metadata for:
    1. Required metadata for DataCite4.1 (see template)
    2. Adequate description of data in the description field
        - Since this is a single line, it is hard to read. The suggestion is to copy the text to a text file and review it there. If you make edits, copy it back to the field.
    3. Any errors. 
        - Common errors include:
            1. Putting all the subjects in a single comma-separated list instead of each subject in its own field
            2. Entering something in the year or identifier field. This will cause DOI creation to fail.
4. If metadata or data do not pass review, contact submitter to make revisions. Make sure they have access to their folder in the staging area (see note about permissions on the [PID Curator Page](https://cyverse.atlassian.net/wiki/spaces/DC/pages/241867456/PID+Curator+Page)).
5. Once data and metadata pass review, change status to *Approved*. In the comment field, note any changes that were made or special accommodations.

#### Post evaluation

1. Create the identifier:
    1. In the DE admin tab, under DOI requests, select the request, then click on Create Permanent Identifier.
    2. After a short wait, you should receive a popup notification that the ID was created, or an error message, if there was some problem.
    3. If you view the metadata, you should now see a DOI in the Identifier field.
    4. Status should change automatically to Completion.
2. Throughout the process, the user will receive notifications via the DE of any change in status. They only receive email for the initial request and final creation.
3. When the DOI is created:
    1. The user and curators will receive an email notification with the DOI.
    2. The folder will be moved out of the staging area and into the curated area of the data_commons folder.
    3. The dataset will be visible in the Data Commons Repository (https://datacommons.cyverse.org/ or https://dc.cyverse.org/).

!!! Warning "Achtung!"

    Due to a known bug, the citation for the DOI will not display correctly on the landing page unless you take the following additional step after DOI creation:

    - Add identifierType metadata:
        1. Open the metadata for the folder in /iplant/home/shared/commons_repo/curated
        2. Click to add a new AVU
        3. change the Attribute to "identiferType"
        4. change the Value to "DOI"
        5. there should be no unit
        6. save the metadata

!!! Note "Managing permissions"

    In order for DOI requestors to see their data under `/iplant/home/share/staging`, they need read access to that folder. The way this is currently done is to give them recursive read permission to `/iplant/home/shared/commons_repo`. Due to a known problem, once this happens, they are subsequently given read permission to any new DOI request folders under staging. To avoid this problem, please complete the following additional step:

    - Remove user's read permission from `/iplant/home/shared/commons_repo`. This can be done easily in the DE or using iCommands.

#### Updating a DOI dataset

Once a DOI is has been created, the data should not change. However, if the user notices a small mistake within a day or two, and the dataset has not been publicized, it is generally safe to change, especially if the change is an addition. Generally, any change to the data contained within a DOI dataset requires issuing a new DOI as a new version of the dataset.

##### Creating a new verson

New version of a dataset may be published due to updates or mistakes in the original dataset.

1. If the new version is due to a mistake, have the user copy the old version to their home directory, make the needed changes, and then submit a new DOI request as per usual. Otherwise, just have them request a new DOI. They should fill in the Version field.
2. Be sure to include a link to the previous version in the new version's metadata using the `relatedIdentifier` field. 
3. Create the new DOI
4. Edit metadata of the old DOI:
    1. Point to the new version using the `relatedIdentifier` field. 
    2. Add a link to the new version of the dataset using the `relatedIdentifier` field. 
    3. Check the "deprecated" field in the DataCite metadata template, if the new version is due to a mistake in the old version
    4. If the dataset is deprecated, change the title of the dataset to include the word deprecated. You may want have the user edit the dataset description as well.

##### Updating metadata

You can update the metadata for DOI datasets after creation, but changes generally should be only to correct errors or add more information. The most important use case is to add the DOI of a related publication that comes out later, using the `relatedIdentifier` field.

!!! Note "Remember!"
    **Any changes you make to the metadata in the DE must also be synced with DataCite**. This can only be done manually by accessing Fabrica.

#### Nested DOIs

!!! Warning
    This section is subject to changes.

For complex datasets, there may be one overarching DOI, plus additional DOIs for folders within the Dataset. This process requires some manual curation and coordination with the requestor. 

1. User should contact doi@cyverse.org to discuss this before requesting their DOIs.
2. User creates a folder for each sub-folder (i.e. each granular DOI), adds metadata, and requests DOI as per usual.
3. User creates the top level folder for the entire dataset and requests a DOI
    1. Must include the usual metadata plus a readme file that lists all of the sub-DOIs
    2. May include additional data files, beyond the sub-folders
    3. DOES NOT include any of the sub-folders at this point.
4. Curator creates DOIs for all the sub-DOIs as per normal.
5. Curator creates DOI for top level folder as per normal
6. Curator moves all sub-folders into the top level folder
    1. This can be done in DE or with iCommands
7. Curator updates locations of all sub-DOI datasets in Fabrica
8. Curator edits the metadata of each subfolder to include a link to the parent DOI using the "relatedIdentifier" field. 
9. Curator edits the metadata of the top level folder to include links to all the child DOIs using the "relatedIdentifier" field. Each child should get its own metadatum.

#### Known potential issues

!!! Warning
    This section is subject to changes.

##### DOIs for datasets with >1000 files

!!! Warning
    This issue might have been resolved since first reported.

If the dataset for which a DOI is requested contains more than 1000 objects (files or folders), the DE cannot currently move it. You and the submitter will get an email notice to this effect. The dataset can be moved manually using iRODS. Please follow the following steps.

1. Contact submitter and ask if they can generate tallballs of the directories/files. Data Store handles large files much better than lots of small files. 
2. Curators receive an email notification that a DOI has been requested via the DE and that the folder could not be moved.
3. Contact the DOI requestor. Their user name is in the DOI request, and you can look up their email at https://user.cyverse.org/. Ask them to make you an owner of the folder.
4. Using iCommands, move the folder to `/iplant/home/shared/staging/$foldername`
5. Curate as usual. Upon creation, you will again receive a notice that the folder could not be moved.
6. Using iCommands, move the folder to `/iplant/home/shared/curated/$foldername`
7. Using iCommands, set the permissions on the folder so that "public" and "anonymous" have read permission. Remove any other permissions EXCEPT for DOI-curators – keep those.
    1. Example: `ichmod -rV read anonymous Miao_Schnable_sorghumHighThroughputPhenotyping_2017 `

If you follow the steps above, our system will be able to track the location of the folder properly. However, it does mean you have to manually move the data twice. There are other options. Talk to Paul or Ramona if you run into problems.

##### DOIs and keeping data residing in their normal place

1. User requests the DOI as normal.
2. Curator creates the DOI as normal. This puts the final datasets with all metadata in `/iplant/home/shared/commons_repo/curated`
3. Curator requests write permission to the folder where the final dataset will live (in this case, `/iplant/home/shared/iVirus`) for both themselves and the user DOI-curators. 

    !!! Note 
        External links are currently only for other folders within `/iplant/home/shared`. This could change, but then this SOP would need to change.

4. Curator copies final dataset from `/iplant/home/shared/commons_repo/curated` to the new location. 
5. Curator uses metadata copy function to copy the metadata from the location in `/iplant/home/shared/commons_repo/curated` to the new location.
6. Curator changes permission in new folder so that only themselves and DOI-curators have write and own permission (DOI-curators is an iRODS group and permissions can only be assigned with iCommands. Everyone else's inherited permissions should be changed to read or removed.
7. Curator adds read permissions to the new folder for users 'Public User' and 'Anonymous-Public Access'.
8. Curator removes all files from the original dataset (in `/iplant/home/shared/commons_repo/curated`) except the readme file.
9. Curator edits the metadata of the folder in `/iplant/home/shared/commons_repo/curated` to include a new attribute for `external_data_link` (must be spelled exactly like this to work). The value for that field should be the Data Commons link to the new copy of the dataset. For an example, see this [metadata file](https://datacommons.cyverse.org/browse/iplant/home/shared/commons_repo/curated/Howard-Varona_and_Vik_et_al_Phage_therapy_2018). Once this is done, a link to the other location will automatically show up on the DC website, after it refreshes. 
10. Update URL for the DOI at DataCite Fabrica.

### Reference Genomes

Hosted reference genomes for featured genomic analyses

### Subscriptions

View/Modify User account subscriptions for CPU hours and data storage capacity

### Tools

Create/Edit/Deprecate K8s templates for public Docker images hosted on public registries

### VICE Access

Grant access to interactive (K8s) run applications 

Limited access to verified accounts only.
