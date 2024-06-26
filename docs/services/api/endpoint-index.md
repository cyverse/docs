**Jump to:**

[`/admin`](#admin)

[`/apps`](#apps)

[`/coge`](#coge)

[`/favorites`](#favorites)

[`/filesystem`](#filesystem)

[`/permanent-id-requests`](#permanent-id-requests)

[`/secured`](#secured)

[`/send-notification`](#send-notification)

[`/uuid`](#uuid)

## get

[`GET /`](endpoints/misc.md#verifying-that-terrain-is-running)

## admin

[`GET /admin/apps/categories`](endpoints/app-metadata.md#listing-app-categories) 

[`GET /admin/apps/categories/search`](endpoints/app-metadata.md#searching-for-categories-by-name) 

[`POST /admin/apps/categories/{system-id}`](endpoints/app-metadata.md#adding-categories) 

[`DELETE /admin/apps/categories/{system-id}/{category-id}`](endpoints/app-metadata.md#deleting-a-category-by-id) 

[`PATCH /admin/apps/categories/{system-id}/{category-id}`](endpoints/app-metadata.md#updating-an-app-category) 

[`DELETE /admin/apps/{app-id}/comments/{comment-id}`](endpoints/comments.md#administratively-deleting-a-comment) 

[`PATCH /admin/apps/{app-id}/comments/{comment-id}`](endpoints/comments.md#retractingreadmitting-a-comment) 

[`GET /admin/apps/{app-id}/metadata`](endpoints/app-metadata.md#managing-app-avu-metadata) 

[`POST /admin/apps/{app-id}/metadata`](endpoints/app-metadata.md#managing-app-avu-metadata) 

[`PUT /admin/apps/{app-id}/metadata`](endpoints/app-metadata.md#managing-app-avu-metadata) 

[`DELETE /admin/filesystem/entry/{entry-id}/comments/{comment-id}`](endpoints/comments.md#administratively-deleting-a-comment) 

[`PATCH /admin/filesystem/entry/{entry-id}/comments/{comment-id}`](endpoints/comments.md#retractingreadmitting-a-comment) 

[`GET /admin/filesystem/metadata/templates`](endpoints/filesystem/metadata.md#listing-metadata-templates) 

[`POST /admin/filesystem/metadata/templates`](endpoints/filesystem/metadata.md#adding-metadata-templates) 

[`DELETE /admin/filesystem/metadata/templates/{template-id}`](endpoints/filesystem/metadata.md#marking-a-metadata-template-as-deleted) 

[`POST /admin/filesystem/metadata/templates/{template-id}`](endpoints/filesystem/metadata.md#updating-metadata-templates) 

[`GET /admin/notifications/system`](endpoints/notifications.md#endpoints-for-system-messages-aka-system-notifications) 

[`PUT /admin/notifications/system`](endpoints/notifications.md#endpoints-for-system-messages-aka-system-notifications) 

[`GET /admin/notifications/system-types`](endpoints/notifications.md#endpoints-for-system-messages-aka-system-notifications) 

[`DELETE /admin/notifications/system/:uuid`](endpoints/notifications.md#endpoints-for-system-messages-aka-system-notifications) 

[`GET /admin/notifications/system/:uuid`](endpoints/notifications.md#endpoints-for-system-messages-aka-system-notifications) 

[`POST /admin/notifications/system/:uuid`](endpoints/notifications.md#endpoints-for-system-messages-aka-system-notifications) 

[`GET /admin/ontologies`](endpoints/app-ontologies.md#listing-saved-ontology-details) 

[`POST /admin/ontologies`](endpoints/app-ontologies.md#save-an-ontology-xml-document) 

[`DELETE /admin/ontologies/{ontology-version}`](endpoints/app-ontologies.md#logically-deleting-an-ontology) 

[`GET /admin/ontologies/{ontology-version}`](endpoints/app-ontologies.md#listing-hierarchies-for-any-ontology) 

[`POST /admin/ontologies/{ontology-version}`](endpoints/app-ontologies.md#set-active-ontology-version) 

[`DELETE /admin/ontologies/{ontology-version}/{root-iri}`](endpoints/app-ontologies.md#deleting-an-ontology-hierarchy) 

[`GET /admin/ontologies/{ontology-version}/{root-iri}`](endpoints/app-ontologies.md#listing-filtered-hierarchies-for-any-ontology) 

[`PUT /admin/ontologies/{ontology-version}/{root-iri}`](endpoints/app-ontologies.md#save-an-ontology-hierarchy) 

[`GET /admin/ontologies/{ontology-version}/{root-iri}/apps`](endpoints/app-ontologies.md#listing-apps-in-hierarchies-for-any-ontology) 

[`GET /admin/ontologies/{ontology-version}/{root-iri}/unclassified`](endpoints/app-ontologies.md#listing-unclassified-apps-for-any-ontology) 

[`GET /admin/permanent-id-requests`](endpoints/permanent-id-requests.md#list-all-permanent-id-requests) 

[`GET /admin/permanent-id-requests/{request-id}`](endpoints/permanent-id-requests.md#get-any-permanent-id-request-details) 

[`POST /admin/permanent-id-requests/{request-id}/ezid`](endpoints/permanent-id-requests.md#create-a-permanent-id) 

[`POST /admin/permanent-id-requests/{request-id}/status`](endpoints/permanent-id-requests.md#update-the-status-of-a-permanent-id-request) 

[`DELETE /admin/workspaces`](endpoints/app-metadata.md#deleting-workspaces) 

[`GET /admin/workspaces`](endpoints/app-metadata.md#listing-workspaces) 

## apps

[`GET /apps/{app-id}/comments`](endpoints/comments.md#listing-comments) 

[`POST /apps/{app-id}/comments`](endpoints/comments.md#creating-a-comment) 

[`PATCH /apps/{app-id}/comments/{comment-id}`](endpoints/comments.md#retractingreadmitting-a-comment) 

[`PATCH /apps/{app-id}/comments/{comment-id}`](endpoints/comments.md#retractingreadmitting-a-comment) 

## coge

[`GET /coge/genomes`](endpoints/filesystem/coge.md#searching-for-genomes-in-coge) 

[`POST /coge/genomes/load`](endpoints/filesystem/coge.md#viewing-a-genome-file-in-coge) 

[`POST /coge/genomes/{genome-id}/export-fasta`](endpoints/filesystem/coge.md#exporting-coge-genome-data-to-irods) 

## favorites

[`GET /favorites/filesystem`](endpoints/favorites.md#listing-stat-info-for-favorite-data) 

## filesystem

[`PATCH /filesystem/entry/{entry-id}/comments/{comment-id}`](endpoints/comments.md#retractingreadmitting-a-comment) 

## permanent-id-requests

[`GET /permanent-id-requests`](endpoints/permanent-id-requests.md#list-permanent-id-requests) 

[`POST /permanent-id-requests`](endpoints/permanent-id-requests.md#create-a-permanent-id-request) 

[`GET /permanent-id-requests/status-codes`](endpoints/permanent-id-requests.md#list-permanent-id-request-status-codes) 

[`GET /permanent-id-requests/types`](endpoints/permanent-id-requests.md#list-permanent-id-request-types) 

[`GET /permanent-id-requests/{request-id}`](endpoints/permanent-id-requests.md#list-permanent-id-request-details) 

## secured

[`GET /secured/favorites/filesystem`](endpoints/favorites.md#listing-stat-info-for-favorite-data) 

[`DELETE /secured/favorites/filesystem/{favorite}`](endpoints/favorites.md#removing-a-data-resource-from-being-a-favorite) 

[`PUT /secured/favorites/filesystem/{favorite}`](endpoints/favorites.md#marking-a-data-resource-as-favorite) 

[`POST /secured/favorites/filter`](endpoints/favorites.md#filter-a-set-of-resources-for-favorites) 

[`GET /secured/fileio/download`](endpoints/fileio.md#downloading) 

[`POST /secured/fileio/save`](endpoints/fileio.md#save) 

[`POST /secured/fileio/saveas`](endpoints/fileio.md#save-as) 

[`POST /secured/fileio/upload`](endpoints/fileio.md#uploading) 

[`POST /secured/fileio/urlupload`](endpoints/fileio.md#url-uploads) 

[`POST /secured/filesystem/delete`](endpoints/filesystem/delete.md#deleting-files-andor-directories) 

[`POST /secured/filesystem/delete-contents`](endpoints/filesystem/delete.md#deleting-all-items-in-a-directory) 

[`POST /secured/filesystem/delete-tickets`](endpoints/filesystem/tickets.md#deleting-tickets) 

[`POST /secured/filesystem/directories`](endpoints/filesystem/directory-create.md#batch-directory-creation) 

[`GET /secured/filesystem/directory`](endpoints/filesystem/directory-listing.md#directory-list-non-recursive) 

[`POST /secured/filesystem/directory/create`](endpoints/filesystem/directory-create.md#directory-creation) 

[`GET /secured/filesystem/display-download`](endpoints/fileio.md#downloading) 

[`GET /secured/filesystem/entry/{entry-id}/comments`](endpoints/comments.md#listing-comments) 

[`POST /secured/filesystem/entry/{entry-id}/comments`](endpoints/comments.md#creating-a-comment) 

[`PATCH /secured/filesystem/entry/{entry-id}/comments/{comment-id}`](endpoints/comments.md#retractingreadmitting-a-comment) 

[`POST /secured/filesystem/exists`](endpoints/filesystem/existence.md#filedirectory-existence) 

[`GET /secured/filesystem/file/manifest`](endpoints/filesystem/manifest.md#file-manifest) 

[`GET /secured/filesystem/index`](endpoints/filesystem/search.md#endpoints) 

[`POST /secured/filesystem/list-tickets`](endpoints/filesystem/tickets.md#listing-tickets) 

[`POST /secured/filesystem/metadata/csv-parser`](endpoints/filesystem/metadata.md#adding-batch-metadata-to-multiple-paths-from-a-csv-file) 

[`GET /secured/filesystem/metadata/template/attr/{attribute-id}`](endpoints/filesystem/metadata.md#viewing-a-metadata-attribute) 

[`GET /secured/filesystem/metadata/template/{template-id}`](endpoints/filesystem/metadata.md#viewing-a-metadata-template) 

[`GET /secured/filesystem/metadata/template/{template-id}/blank-csv`](endpoints/filesystem/metadata.md#downloading-a-blank-template) 

[`GET /secured/filesystem/metadata/template/{template-id}/guide-csv`](endpoints/filesystem/metadata.md#downloading-a-template-guide) 

[`GET /secured/filesystem/metadata/templates`](endpoints/filesystem/metadata.md#listing-metadata-templates) 

[`POST /secured/filesystem/move`](endpoints/filesystem/move.md#moving-files-andor-directories) 

[`POST /secured/filesystem/move-contents`](endpoints/filesystem/move.md#moving-all-items-in-a-directory) 

[`GET /secured/filesystem/paged-directory`](endpoints/filesystem/directory-listing.md#paged-directory-listing) 

[`POST /secured/filesystem/path-list-creator`](endpoints/filesystem/path-lists.md#ht-path-list-creator` 

[`POST /secured/filesystem/read-chunk`](endpoints/filesystem/read-chunk.md#reading-a-chunk-of-a-file) 

[`POST /secured/filesystem/read-csv-chunk`](endpoints/filesystem/csv-tsv-parsing.md#csvtsv-parsing) 

[`POST /secured/filesystem/rename`](endpoints/filesystem/rename.md#renaming-a-file-or-directory) 

[`POST /secured/filesystem/restore`](endpoints/filesystem/restore.md#restoring-a-file-or-directory-from-a-users-trash` 

[`POST /secured/filesystem/restore-all`](endpoints/filesystem/restore.md#restoring-all-items-in-a-users-trash` 

[`GET /secured/filesystem/root`](endpoints/filesystem/root-listing.md#top-level-root-listing) 

[`POST /secured/filesystem/stat`](endpoints/filesystem/stat.md#file-and-directory-status-information) 

[`POST /secured/filesystem/tickets`](endpoints/filesystem/tickets.md#creating-tickets) 

[`DELETE /secured/filesystem/trash`](endpoints/filesystem/empty-trash.md#emptying-a-users-trash-directory) 

[`POST /secured/filesystem/user-permissions`](endpoints/filesystem/permissions.md#listing-user-permissions) 

[`GET /secured/filesystem/{data-id}/metadata`](endpoints/filesystem/metadata.md#getting-metadata) 

[`POST /secured/filesystem/{data-id}/metadata`](endpoints/filesystem/metadata.md#setting-metadata) 

[`POST /secured/filesystem/{data-id}/metadata/copy`](endpoints/filesystem/metadata.md#copying-all-metadata-from-a-filefolder` 

[`POST /secured/filesystem/{data-id}/metadata/save`](endpoints/filesystem/metadata.md#exporting-metadata-to-a-file) 

[`POST /secured/filesystem/{data-id}/ore/save`](endpoints/filesystem/ore.md#generating-oai-ore-files-for-a-data-set) 

[`GET /secured/notifications/count-messages`](endpoints/notifications.md#obtaining-notification-counts) 

[`POST /secured/notifications/delete`](endpoints/notifications.md#marking-notifications-as-deleted) 

[`DELETE /secured/notifications/delete-all`](endpoints/notifications.md#marking-all-notifications-as-deleted) 

[`GET /secured/notifications/last-ten-messages`](endpoints/notifications.md#obtaining-the-ten-most-recent-notifications) 

[`POST /secured/notifications/mark-all-seen`](endpoints/notifications.md#marking-all-notifications-as-seen) 

[`GET /secured/notifications/messages`](endpoints/notifications.md#obtaining-notifications) 

[`POST /secured/notifications/seen`](endpoints/notifications.md#marking-notifications-as-seen) 

[`POST /secured/notifications/system/delete`](endpoints/notifications.md#endpoints-for-system-messages-aka-system-notifications) 

[`DELETE /secured/notifications/system/delete-all`](endpoints/notifications.md#endpoints-for-system-messages-aka-system-notifications) 

[`POST /secured/notifications/system/mark-all-received`](endpoints/notifications.md#endpoints-for-system-messages-aka-system-notifications) 

[`POST /secured/notifications/system/mark-all-seen`](endpoints/notifications.md#endpoints-for-system-messages-aka-system-notifications) 

[`GET /secured/notifications/system/messages`](endpoints/notifications.md#endpoints-for-system-messages-aka-system-notifications) 

[`GET /secured/notifications/system/new-messages`](endpoints/notifications.md#endpoints-for-system-messages-aka-system-notifications) 

[`POST /secured/notifications/system/received`](endpoints/notifications.md#endpoints-for-system-messages-aka-system-notifications) 

[`POST /secured/notifications/system/seen`](endpoints/notifications.md#endpoints-for-system-messages-aka-system-notifications) 

[`GET /secured/notifications/system/unseen-messages`](endpoints/notifications.md#endpoints-for-system-messages-aka-system-notifications) 

[`GET /secured/notifications/unseen-messages`](endpoints/notifications.md#obtaining-unseen-notifications) 

[`GET /secured/oauth/access-code/{api-name}`](endpoints/callbacks.md#obtaining-oauth-authorization-codes) 

[`DELETE /secured/preferences`](endpoints/misc.md#removing-user-preferences) 

[`GET /secured/preferences`](endpoints/misc.md#retrieving-user-preferences) 

[`POST /secured/preferences`](endpoints/misc.md#saving-user-preferences) 

[`DELETE /secured/sessions`](endpoints/misc.md#removing-user-session-data) 

[`GET /secured/sessions`](endpoints/misc.md#retrieving-user-session-data) 

[`POST /secured/sessions`](endpoints/misc.md#saving-user-session-data) 

## send-notification

[`POST /send-notification.`](endpoints/notifications.md#sending-an-arbitrary-notification) 

## uuid

[`GET /uuid`](endpoints/misc.md#obtaining-identifiers) 

