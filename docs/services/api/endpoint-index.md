**Jump to:**

[`/admin`](#admin)

[`/apps`](#apps)

[`/coge`](#coge)

[`/collaborator-lists`](#collaborator-lists)

[`/export-workflow`](#export-workflow)

[`/favorites`](#favorites)

[`/filesystem`](#filesystem)

[`/permanent-id-requests`](#permanent-id-requests)

[`/secured`](#secured)

[`/send-notification`](#send-notification)

[`/subjects`](#subjects)

[`/support-email`](#support-email)

[`/teams`](#teams)

[`/tool-requests`](#tool-requests)

[`/uuid`](#uuid)

## get

[`GET /`](endpoints/misc.md#verifying-that-terrain-is-running)

## admin

[`GET /admin/apps`](endpoints/app-metadata.md#searching-for-apps) 

[`POST /admin/apps`](endpoints/app-metadata.md#categorizing-apps) 

[`GET /admin/apps/categories`](endpoints/app-metadata.md#listing-app-categories) 

[`POST /admin/apps/categories`](endpoints/app-metadata.md#adding-categories) 

[`GET /admin/apps/categories/search`](endpoints/app-metadata.md#searching-for-categories-by-name) 

[`POST /admin/apps/categories/shredder`](endpoints/app-metadata.md#deleting-categories) 

[`DELETE /admin/apps/categories/{category-id}`](endpoints/app-metadata.md#deleting-a-category-by-id) 

[`PATCH /admin/apps/categories/{category-id}`](endpoints/app-metadata.md#updating-an-app-category) 

[`POST /admin/apps/shredder`](endpoints/app-metadata.md#permanently-deleting-an-app) 

[`DELETE /admin/apps/{app-id}`](endpoints/app-metadata.md#logically-deleting-apps) 

[`PATCH /admin/apps/{app-id}`](endpoints/app-metadata.md#updating-app-labels) 

[`DELETE /admin/apps/{app-id}/comments/{comment-id}`](endpoints/comments.md#administratively-deleting-a-comment) 

[`PATCH /admin/apps/{app-id}/comments/{comment-id}`](endpoints/comments.md#retractingreadmitting-a-comment) 

[`PATCH /admin/apps/{app-id}/documentation`](endpoints/app-metadata.md#app-documentation) 

[`POST /admin/apps/{app-id}/documentation`](endpoints/app-metadata.md#app-documentation) 

[`GET /admin/apps/{app-id}/metadata`](endpoints/app-metadata.md#managing-app-avu-metadata) 

[`POST /admin/apps/{app-id}/metadata`](endpoints/app-metadata.md#managing-app-avu-metadata) 

[`PUT /admin/apps/{app-id}/metadata`](endpoints/app-metadata.md#managing-app-avu-metadata) 

[`GET /admin/config`](endpoints/admin.md#listing-the-config-for-terrain) 

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

[`GET /admin/status`](endpoints/admin.md#status-check` 

[`GET /admin/tool-requests`](endpoints/app-metadata.md#listing-tool-installation-requests) 

[`GET /admin/tool-requests/{tool-request-id}`](endpoints/app-metadata.md#listing-tool-installation-request-details) 

[`POST /admin/tool-requests/{tool-request-id}/status`](endpoints/app-metadata.md#updating-a-tool-installation-request) 

[`DELETE /admin/workspaces`](endpoints/app-metadata.md#deleting-workspaces) 

[`GET /admin/workspaces`](endpoints/app-metadata.md#listing-workspaces) 

## apps

[`POST /apps`](endpoints/app-metadata.md#creating-an-app-for-the-current-user` 

[`POST /apps/:app-id/publish`](endpoints/app-metadata.md#submitting-an-app-for-public-use) 

[`POST /apps/arg-preview`](endpoints/app-metadata.md#previewing-command-line-arguments) 

[`GET /apps/categories`](endpoints/app-metadata.md#listing-app-categories) 

[`GET /apps/categories/{group-id}`](endpoints/app-metadata.md#listing-apps-in-an-app-group) 

[`GET /apps/elements`](endpoints/app-metadata.md#listing-app-elements) 

[`GET /apps/elements/{element-type}`](endpoints/app-metadata.md#listing-app-elements) 

[`GET /apps/hierarchies`](endpoints/app-ontologies.md#listing-ontology-hierarchies) 

[`GET /apps/hierarchies/{root-iri}`](endpoints/app-ontologies.md#listing-filtered-ontology-hierarchies) 

[`GET /apps/hierarchies/{root-iri}/apps`](endpoints/app-ontologies.md#listing-apps-in-hierarchies-for-the-active-ontology) 

[`GET /apps/hierarchies/{root-iri}/unclassified`](endpoints/app-ontologies.md#listing-unclassified-apps-for-the-active-ontology) 

[`GET /apps/ids`](endpoints/app-metadata.md#listing-app-identifiers) 

[`POST /apps/permission-lister`](endpoints/app-metadata.md#listing-permissions-for-a-set-of-apps) 

[`POST /apps/pipelines`](endpoints/app-metadata.md#creating-a-pipeline) 

[`PUT /apps/pipelines/{app-id}`](endpoints/app-metadata.md#updating-a-pipeline) 

[`POST /apps/pipelines/{app-id}/copy`](endpoints/app-metadata.md#making-a-copy-of-a-pipeline-available-for-editing) 

[`GET /apps/pipelines/{app-id}/ui`](endpoints/app-metadata.md#making-a-pipeline-available-for-editing) 

[`POST /apps/share`](endpoints/app-metadata.md#granting-access-to-a-set-of-apps) 

[`POST /apps/shredder`](endpoints/app-metadata.md#logically-deleting-apps) 

[`POST /apps/unshare`](endpoints/app-metadata.md#revoking-access-to-a-set-of-apps) 

[`DELETE /apps/{app-id}`](endpoints/app-metadata.md#logically-deleting-apps) 

[`GET /apps/{app-id}`](endpoints/app-metadata.md#getting-analyses-in-the-json-format-required-by-the-de) 

[`PATCH /apps/{app-id}`](endpoints/app-metadata.md#updating-app-labels) 

[`PUT /apps/{app-id}`](endpoints/app-metadata.md#updating-a-single-step-app) 

[`GET /apps/{app-id}/comments`](endpoints/comments.md#listing-comments) 

[`POST /apps/{app-id}/comments`](endpoints/comments.md#creating-a-comment) 

[`PATCH /apps/{app-id}/comments/{comment-id}`](endpoints/comments.md#retractingreadmitting-a-comment) 

[`PATCH /apps/{app-id}/comments/{comment-id}`](endpoints/comments.md#retractingreadmitting-a-comment) 

[`POST /apps/{app-id}/copy`](endpoints/app-metadata.md#making-a-copy-of-an-app-available-for-editing) 

[`GET /apps/{app-id}/details`](endpoints/app-metadata.md#getting-app-details) 

[`GET /apps/{app-id}/documentation`](endpoints/app-metadata.md#app-documentation) 

[`PATCH /apps/{app-id}/documentation`](endpoints/app-metadata.md#app-documentation) 

[`POST /apps/{app-id}/documentation`](endpoints/app-metadata.md#app-documentation) 

[`DELETE /apps/{app-id}/favorite`](endpoints/app-metadata.md#removing-an-app-favorite) 

[`PUT /apps/{app-id}/favorite`](endpoints/app-metadata.md#adding-an-app-favorite) 

[`GET /apps/{app-id}/is-publishable`](endpoints/app-metadata.md#determining-if-an-app-can-be-made-public) 

[`GET /apps/{app-id}/metadata`](endpoints/app-metadata.md#managing-app-avu-metadata) 

[`POST /apps/{app-id}/metadata`](endpoints/app-metadata.md#managing-app-avu-metadata) 

[`PUT /apps/{app-id}/metadata`](endpoints/app-metadata.md#managing-app-avu-metadata) 

[`DELETE /apps/{app-id}/rating`](endpoints/app-metadata.md#deleting-app-ratings) 

[`POST /apps/{app-id}/rating`](endpoints/app-metadata.md#rating-apps) 

[`GET /apps/{app-id}/tasks`](endpoints/app-metadata.md#listing-tasks-in-an-app) 

[`GET /apps/{app-id}/tools`](endpoints/app-metadata.md#listing-tools-in-an-app) 

[`GET /apps/{app-id}/ui`](endpoints/app-metadata.md#obtaining-an-app-representation-for-editing) 

[`GET /apps?search={term}`](endpoints/app-metadata.md#searching-for-apps) 

## coge

[`GET /coge/genomes`](endpoints/filesystem/coge.md#searching-for-genomes-in-coge) 

[`POST /coge/genomes/load`](endpoints/filesystem/coge.md#viewing-a-genome-file-in-coge) 

[`POST /coge/genomes/{genome-id}/export-fasta`](endpoints/filesystem/coge.md#exporting-coge-genome-data-to-irods) 

## collaborator-lists

[`GET /collaborator-lists`](endpoints/collaborator-lists.md#listing-collaborator-lists) 

[`POST /collaborator-lists`](endpoints/collaborator-lists.md#adding-a-collaborator-list) 

[`DELETE /collaborator-lists/{name}`](endpoints/collaborator-lists.md#deleting-a-collaborator-list) 

[`GET /collaborator-lists/{name}`](endpoints/collaborator-lists.md#getting-information-about-a-collaborator-list) 

[`PATCH /collaborator-lists/{name}`](endpoints/collaborator-lists.md#updating-a-collaborator-list) 

[`GET /collaborator-lists/{name}/members`](endpoints/collaborator-lists.md#listing-collaborator-list-members) 

[`POST /collaborator-lists/{name}/members`](endpoints/collaborator-lists.md#adding-collaborator-list-members) 

[`POST /collaborator-lists/{name}/members/deleter`](endpoints/collaborator-lists.md#removing-collaborator-list-members) 

## export-workflow

[`GET /export-workflow/{analysis-id}`](endpoints/app-metadata.md#exporting-an-analysis) 

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

[`GET /secured/bootstrap`](endpoints/misc.md#initializing-a-users-workspace-and-preferences) 

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

[`POST /secured/filetypes/type`](endpoints/filetypes.md#addupdateunset-a-file-type-of-a-file) 

[`GET /secured/filetypes/type-list`](endpoints/filetypes.md#get-the-list-of-supported-file-types) 

[`GET /secured/logout`](endpoints/misc.md#recording-when-a-user-logs-out) 

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

[`DELETE /secured/saved-searches`](endpoints/misc.md#deleting-saved-searches) 

[`GET /secured/saved-searches`](endpoints/misc.md#getting-saved-searches) 

[`POST /secured/saved-searches`](endpoints/misc.md#setting-saved-searches) 

[`DELETE /secured/sessions`](endpoints/misc.md#removing-user-session-data) 

[`GET /secured/sessions`](endpoints/misc.md#retrieving-user-session-data) 

[`POST /secured/sessions`](endpoints/misc.md#saving-user-session-data) 

## send-notification

[`POST /send-notification.`](endpoints/notifications.md#sending-an-arbitrary-notification) 

## subjects

[`GET /subjects`](endpoints/subjects.md#search-for-subjects) 

## support-email

[`POST /support-email`](endpoints/misc.md#requesting-support) 

## teams

[`GET /teams`](endpoints/teams.md#listing-teams) 

[`POST /teams`](endpoints/teams.md#creating-a-new-team` 

[`DELETE /teams/{name}`](endpoints/teams.md#deleting-a-team` 

[`GET /teams/{name}`](endpoints/teams.md#getting-team-information) 

[`PATCH /teams/{name}`](endpoints/teams.md#updating-a-team` 

[`POST /teams/{name}/join`](endpoints/teams.md#joining-a-team` 

[`POST /teams/{name}/join-request`](endpoints/teams.md#requesting-to-join-a-team` 

[`POST /teams/{name}/join-request/{requester}/deny`](endpoints/teams.md#denying-a-request-to-join-a-team` 

[`POST /teams/{name}/leave`](endpoints/teams.md#leaving-a-team` 

[`GET /teams/{name}/members`](endpoints/teams.md#listing-team-members) 

[`POST /teams/{name}/members`](endpoints/teams.md#adding-team-members) 

[`POST /teams/{name}/members/deleter`](endpoints/teams.md#removing-team-members) 

[`GET /teams/{name}/privileges`](endpoints/teams.md#listing-team-privileges) 

[`POST /teams/{name}/privileges`](endpoints/teams.md#updating-team-privileges) 

## tool-requests

[`GET /tool-requests`](endpoints/app-metadata.md#listing-tool-installation-requests) 

[`POST /tool-requests`](endpoints/app-metadata.md#requesting-installation-of-a-tool` 

[`GET /tool-requests/status-codes`](endpoints/app-metadata.md#listing-tool-request-status-codes) 

## uuid

[`GET /uuid`](endpoints/misc.md#obtaining-identifiers) 

