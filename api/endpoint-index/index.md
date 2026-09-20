---
type: Reference
title: "Endpoint index"
description: "Alphabetical index of every documented Terrain endpoint."
tags: [api, terrain, index]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---
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

[`GET /`](https://docs.cyverse.org/api/endpoints/misc/#verifying-that-terrain-is-running)

## admin

[`GET /admin/apps/categories`](https://docs.cyverse.org/api/endpoints/app-metadata/#listing-app-categories) 

[`GET /admin/apps/categories/search`](https://docs.cyverse.org/api/endpoints/app-metadata/#searching-for-categories-by-name) 

[`POST /admin/apps/categories/{system-id}`](https://docs.cyverse.org/api/endpoints/app-metadata/#adding-categories) 

[`DELETE /admin/apps/categories/{system-id}/{category-id}`](https://docs.cyverse.org/api/endpoints/app-metadata/#deleting-a-category-by-id) 

[`PATCH /admin/apps/categories/{system-id}/{category-id}`](https://docs.cyverse.org/api/endpoints/app-metadata/#updating-an-app-category) 

[`DELETE /admin/apps/{app-id}/comments/{comment-id}`](https://docs.cyverse.org/api/endpoints/comments/#administratively-deleting-a-comment) 

[`PATCH /admin/apps/{app-id}/comments/{comment-id}`](https://docs.cyverse.org/api/endpoints/comments/#retractingreadmitting-a-comment) 

[`GET /admin/apps/{app-id}/metadata`](https://docs.cyverse.org/api/endpoints/app-metadata/#managing-app-avu-metadata) 

[`POST /admin/apps/{app-id}/metadata`](https://docs.cyverse.org/api/endpoints/app-metadata/#managing-app-avu-metadata) 

[`PUT /admin/apps/{app-id}/metadata`](https://docs.cyverse.org/api/endpoints/app-metadata/#managing-app-avu-metadata) 

[`DELETE /admin/filesystem/entry/{entry-id}/comments/{comment-id}`](https://docs.cyverse.org/api/endpoints/comments/#administratively-deleting-a-comment) 

[`PATCH /admin/filesystem/entry/{entry-id}/comments/{comment-id}`](https://docs.cyverse.org/api/endpoints/comments/#retractingreadmitting-a-comment) 

[`GET /admin/filesystem/metadata/templates`](https://docs.cyverse.org/api/endpoints/filesystem/metadata/#listing-metadata-templates) 

[`POST /admin/filesystem/metadata/templates`](https://docs.cyverse.org/api/endpoints/filesystem/metadata/#adding-metadata-templates) 

[`DELETE /admin/filesystem/metadata/templates/{template-id}`](https://docs.cyverse.org/api/endpoints/filesystem/metadata/#marking-a-metadata-template-as-deleted) 

[`POST /admin/filesystem/metadata/templates/{template-id}`](https://docs.cyverse.org/api/endpoints/filesystem/metadata/#updating-metadata-templates) 

[`GET /admin/notifications/system`](https://docs.cyverse.org/api/endpoints/notifications/#endpoints-for-system-messages-aka-system-notifications) 

[`PUT /admin/notifications/system`](https://docs.cyverse.org/api/endpoints/notifications/#endpoints-for-system-messages-aka-system-notifications) 

[`GET /admin/notifications/system-types`](https://docs.cyverse.org/api/endpoints/notifications/#endpoints-for-system-messages-aka-system-notifications) 

[`DELETE /admin/notifications/system/:uuid`](https://docs.cyverse.org/api/endpoints/notifications/#endpoints-for-system-messages-aka-system-notifications) 

[`GET /admin/notifications/system/:uuid`](https://docs.cyverse.org/api/endpoints/notifications/#endpoints-for-system-messages-aka-system-notifications) 

[`POST /admin/notifications/system/:uuid`](https://docs.cyverse.org/api/endpoints/notifications/#endpoints-for-system-messages-aka-system-notifications) 

[`GET /admin/ontologies`](https://docs.cyverse.org/api/endpoints/app-ontologies/#listing-saved-ontology-details) 

[`POST /admin/ontologies`](https://docs.cyverse.org/api/endpoints/app-ontologies/#save-an-ontology-xml-document) 

[`DELETE /admin/ontologies/{ontology-version}`](https://docs.cyverse.org/api/endpoints/app-ontologies/#logically-deleting-an-ontology) 

[`GET /admin/ontologies/{ontology-version}`](https://docs.cyverse.org/api/endpoints/app-ontologies/#listing-hierarchies-for-any-ontology) 

[`POST /admin/ontologies/{ontology-version}`](https://docs.cyverse.org/api/endpoints/app-ontologies/#set-active-ontology-version) 

[`DELETE /admin/ontologies/{ontology-version}/{root-iri}`](https://docs.cyverse.org/api/endpoints/app-ontologies/#deleting-an-ontology-hierarchy) 

[`GET /admin/ontologies/{ontology-version}/{root-iri}`](https://docs.cyverse.org/api/endpoints/app-ontologies/#listing-filtered-hierarchies-for-any-ontology) 

[`PUT /admin/ontologies/{ontology-version}/{root-iri}`](https://docs.cyverse.org/api/endpoints/app-ontologies/#save-an-ontology-hierarchy) 

[`GET /admin/ontologies/{ontology-version}/{root-iri}/apps`](https://docs.cyverse.org/api/endpoints/app-ontologies/#listing-apps-in-hierarchies-for-any-ontology) 

[`GET /admin/ontologies/{ontology-version}/{root-iri}/unclassified`](https://docs.cyverse.org/api/endpoints/app-ontologies/#listing-unclassified-apps-for-any-ontology) 

[`GET /admin/permanent-id-requests`](https://docs.cyverse.org/api/endpoints/permanent-id-requests/) 

[`GET /admin/permanent-id-requests/{request-id}`](https://docs.cyverse.org/api/endpoints/permanent-id-requests/) 

[`POST /admin/permanent-id-requests/{request-id}/ezid`](https://docs.cyverse.org/api/endpoints/permanent-id-requests/) 

[`POST /admin/permanent-id-requests/{request-id}/status`](https://docs.cyverse.org/api/endpoints/permanent-id-requests/) 

[`DELETE /admin/workspaces`](https://docs.cyverse.org/api/endpoints/app-metadata/#deleting-workspaces) 

[`GET /admin/workspaces`](https://docs.cyverse.org/api/endpoints/app-metadata/#listing-workspaces) 

## apps

[`GET /apps/{app-id}/comments`](https://docs.cyverse.org/api/endpoints/comments/#listing-comments) 

[`POST /apps/{app-id}/comments`](https://docs.cyverse.org/api/endpoints/comments/#creating-a-comment) 

[`PATCH /apps/{app-id}/comments/{comment-id}`](https://docs.cyverse.org/api/endpoints/comments/#retractingreadmitting-a-comment) 

[`PATCH /apps/{app-id}/comments/{comment-id}`](https://docs.cyverse.org/api/endpoints/comments/#retractingreadmitting-a-comment) 

## coge

[`GET /coge/genomes`](https://docs.cyverse.org/api/endpoints/filesystem/coge/#searching-for-genomes-in-coge) 

[`POST /coge/genomes/load`](https://docs.cyverse.org/api/endpoints/filesystem/coge/#viewing-a-genome-file-in-coge) 

[`POST /coge/genomes/{genome-id}/export-fasta`](https://docs.cyverse.org/api/endpoints/filesystem/coge/#exporting-coge-genome-data-to-irods) 

## favorites

[`GET /favorites/filesystem`](https://docs.cyverse.org/api/endpoints/favorites/#listing-stat-info-for-favorite-data) 

## filesystem

[`PATCH /filesystem/entry/{entry-id}/comments/{comment-id}`](https://docs.cyverse.org/api/endpoints/comments/#retractingreadmitting-a-comment) 

## permanent-id-requests

[`GET /permanent-id-requests`](https://docs.cyverse.org/api/endpoints/permanent-id-requests/) 

[`POST /permanent-id-requests`](https://docs.cyverse.org/api/endpoints/permanent-id-requests/) 

[`GET /permanent-id-requests/status-codes`](https://docs.cyverse.org/api/endpoints/permanent-id-requests/) 

[`GET /permanent-id-requests/types`](https://docs.cyverse.org/api/endpoints/permanent-id-requests/) 

[`GET /permanent-id-requests/{request-id}`](https://docs.cyverse.org/api/endpoints/permanent-id-requests/) 

## secured

[`GET /secured/favorites/filesystem`](https://docs.cyverse.org/api/endpoints/favorites/#listing-stat-info-for-favorite-data) 

[`DELETE /secured/favorites/filesystem/{favorite}`](https://docs.cyverse.org/api/endpoints/favorites/#removing-a-data-resource-from-being-a-favorite) 

[`PUT /secured/favorites/filesystem/{favorite}`](https://docs.cyverse.org/api/endpoints/favorites/#marking-a-data-resource-as-favorite) 

[`POST /secured/favorites/filter`](https://docs.cyverse.org/api/endpoints/favorites/#filter-a-set-of-resources-for-favorites) 

[`GET /secured/fileio/download`](https://docs.cyverse.org/api/endpoints/fileio/#downloading) 

[`POST /secured/fileio/save`](https://docs.cyverse.org/api/endpoints/fileio/#save) 

[`POST /secured/fileio/saveas`](https://docs.cyverse.org/api/endpoints/fileio/#save-as) 

[`POST /secured/fileio/upload`](https://docs.cyverse.org/api/endpoints/fileio/#uploading) 

[`POST /secured/fileio/urlupload`](https://docs.cyverse.org/api/endpoints/fileio/#url-uploads) 

[`POST /secured/filesystem/delete`](https://docs.cyverse.org/api/endpoints/filesystem/delete/#deleting-files-andor-directories) 

[`POST /secured/filesystem/delete-contents`](https://docs.cyverse.org/api/endpoints/filesystem/delete/#deleting-all-items-in-a-directory) 

[`POST /secured/filesystem/delete-tickets`](https://docs.cyverse.org/api/endpoints/filesystem/tickets/#deleting-tickets) 

[`POST /secured/filesystem/directories`](https://docs.cyverse.org/api/endpoints/filesystem/directory-create/#batch-directory-creation) 

[`GET /secured/filesystem/directory`](https://docs.cyverse.org/api/endpoints/filesystem/directory-listing/#directory-list-non-recursive) 

[`POST /secured/filesystem/directory/create`](https://docs.cyverse.org/api/endpoints/filesystem/directory-create/#directory-creation) 

[`GET /secured/filesystem/display-download`](https://docs.cyverse.org/api/endpoints/fileio/#downloading) 

[`GET /secured/filesystem/entry/{entry-id}/comments`](https://docs.cyverse.org/api/endpoints/comments/#listing-comments) 

[`POST /secured/filesystem/entry/{entry-id}/comments`](https://docs.cyverse.org/api/endpoints/comments/#creating-a-comment) 

[`PATCH /secured/filesystem/entry/{entry-id}/comments/{comment-id}`](https://docs.cyverse.org/api/endpoints/comments/#retractingreadmitting-a-comment) 

[`POST /secured/filesystem/exists`](https://docs.cyverse.org/api/endpoints/filesystem/existence/#filedirectory-existence) 

[`GET /secured/filesystem/file/manifest`](https://docs.cyverse.org/api/endpoints/filesystem/manifest/#file-manifest) 

[`GET /secured/filesystem/index`](https://docs.cyverse.org/api/endpoints/filesystem/search/#endpoints) 

[`POST /secured/filesystem/list-tickets`](https://docs.cyverse.org/api/endpoints/filesystem/tickets/#listing-tickets) 

[`POST /secured/filesystem/metadata/csv-parser`](https://docs.cyverse.org/api/endpoints/filesystem/metadata/#adding-batch-metadata-to-multiple-paths-from-a-csv-file) 

[`GET /secured/filesystem/metadata/template/attr/{attribute-id}`](https://docs.cyverse.org/api/endpoints/filesystem/metadata/#viewing-a-metadata-attribute) 

[`GET /secured/filesystem/metadata/template/{template-id}`](https://docs.cyverse.org/api/endpoints/filesystem/metadata/#viewing-a-metadata-template) 

[`GET /secured/filesystem/metadata/template/{template-id}/blank-csv`](https://docs.cyverse.org/api/endpoints/filesystem/metadata/#downloading-a-blank-template) 

[`GET /secured/filesystem/metadata/template/{template-id}/guide-csv`](https://docs.cyverse.org/api/endpoints/filesystem/metadata/#downloading-a-template-guide) 

[`GET /secured/filesystem/metadata/templates`](https://docs.cyverse.org/api/endpoints/filesystem/metadata/#listing-metadata-templates) 

[`POST /secured/filesystem/move`](https://docs.cyverse.org/api/endpoints/filesystem/move/#moving-files-andor-directories) 

[`POST /secured/filesystem/move-contents`](https://docs.cyverse.org/api/endpoints/filesystem/move/#moving-all-items-in-a-directory) 

[`GET /secured/filesystem/paged-directory`](https://docs.cyverse.org/api/endpoints/filesystem/directory-listing/#paged-directory-listing) 

[`POST /secured/filesystem/path-list-creator`](https://docs.cyverse.org/api/endpoints/filesystem/path-lists/#ht-path-list-creator) 

[`POST /secured/filesystem/read-chunk`](https://docs.cyverse.org/api/endpoints/filesystem/read-chunk/#reading-a-chunk-of-a-file) 

[`POST /secured/filesystem/read-csv-chunk`](https://docs.cyverse.org/api/endpoints/filesystem/csv-tsv-parsing/#csvtsv-parsing) 

[`POST /secured/filesystem/rename`](https://docs.cyverse.org/api/endpoints/filesystem/rename/#renaming-a-file-or-directory) 

[`POST /secured/filesystem/restore`](https://docs.cyverse.org/api/endpoints/filesystem/restore/#restoring-a-file-or-directory-from-a-users-trash) 

[`POST /secured/filesystem/restore-all`](https://docs.cyverse.org/api/endpoints/filesystem/restore/#restoring-all-items-in-a-users-trash) 

[`GET /secured/filesystem/root`](https://docs.cyverse.org/api/endpoints/filesystem/root-listing/#top-level-root-listing) 

[`POST /secured/filesystem/stat`](https://docs.cyverse.org/api/endpoints/filesystem/stat/#file-and-directory-status-information) 

[`POST /secured/filesystem/tickets`](https://docs.cyverse.org/api/endpoints/filesystem/tickets/#creating-tickets) 

[`DELETE /secured/filesystem/trash`](https://docs.cyverse.org/api/endpoints/filesystem/empty-trash/#emptying-a-users-trash-directory) 

[`POST /secured/filesystem/user-permissions`](https://docs.cyverse.org/api/endpoints/filesystem/permissions/#listing-user-permissions) 

[`GET /secured/filesystem/{data-id}/metadata`](https://docs.cyverse.org/api/endpoints/filesystem/metadata/#getting-metadata) 

[`POST /secured/filesystem/{data-id}/metadata`](https://docs.cyverse.org/api/endpoints/filesystem/metadata/#setting-metadata) 

[`POST /secured/filesystem/{data-id}/metadata/copy`](https://docs.cyverse.org/api/endpoints/filesystem/metadata/#copying-all-metadata-from-a-filefolder) 

[`POST /secured/filesystem/{data-id}/metadata/save`](https://docs.cyverse.org/api/endpoints/filesystem/metadata/#exporting-metadata-to-a-file) 

[`POST /secured/filesystem/{data-id}/ore/save`](https://docs.cyverse.org/api/endpoints/filesystem/ore/#generating-oai-ore-files-for-a-data-set) 

[`GET /secured/notifications/count-messages`](https://docs.cyverse.org/api/endpoints/notifications/#obtaining-notification-counts) 

[`POST /secured/notifications/delete`](https://docs.cyverse.org/api/endpoints/notifications/#marking-notifications-as-deleted) 

[`DELETE /secured/notifications/delete-all`](https://docs.cyverse.org/api/endpoints/notifications/#marking-all-notifications-as-deleted) 

[`GET /secured/notifications/last-ten-messages`](https://docs.cyverse.org/api/endpoints/notifications/#obtaining-the-ten-most-recent-notifications) 

[`POST /secured/notifications/mark-all-seen`](https://docs.cyverse.org/api/endpoints/notifications/#marking-all-notifications-as-seen) 

[`GET /secured/notifications/messages`](https://docs.cyverse.org/api/endpoints/notifications/#obtaining-notifications) 

[`POST /secured/notifications/seen`](https://docs.cyverse.org/api/endpoints/notifications/#marking-notifications-as-seen) 

[`POST /secured/notifications/system/delete`](https://docs.cyverse.org/api/endpoints/notifications/#endpoints-for-system-messages-aka-system-notifications) 

[`DELETE /secured/notifications/system/delete-all`](https://docs.cyverse.org/api/endpoints/notifications/#endpoints-for-system-messages-aka-system-notifications) 

[`POST /secured/notifications/system/mark-all-received`](https://docs.cyverse.org/api/endpoints/notifications/#endpoints-for-system-messages-aka-system-notifications) 

[`POST /secured/notifications/system/mark-all-seen`](https://docs.cyverse.org/api/endpoints/notifications/#endpoints-for-system-messages-aka-system-notifications) 

[`GET /secured/notifications/system/messages`](https://docs.cyverse.org/api/endpoints/notifications/#endpoints-for-system-messages-aka-system-notifications) 

[`GET /secured/notifications/system/new-messages`](https://docs.cyverse.org/api/endpoints/notifications/#endpoints-for-system-messages-aka-system-notifications) 

[`POST /secured/notifications/system/received`](https://docs.cyverse.org/api/endpoints/notifications/#endpoints-for-system-messages-aka-system-notifications) 

[`POST /secured/notifications/system/seen`](https://docs.cyverse.org/api/endpoints/notifications/#endpoints-for-system-messages-aka-system-notifications) 

[`GET /secured/notifications/system/unseen-messages`](https://docs.cyverse.org/api/endpoints/notifications/#endpoints-for-system-messages-aka-system-notifications) 

[`GET /secured/notifications/unseen-messages`](https://docs.cyverse.org/api/endpoints/notifications/#obtaining-unseen-notifications) 

[`GET /secured/oauth/access-code/{api-name}`](https://docs.cyverse.org/api/endpoints/callbacks/#obtaining-oauth-authorization-codes) 

[`DELETE /secured/preferences`](https://docs.cyverse.org/api/endpoints/misc/#removing-user-preferences) 

[`GET /secured/preferences`](https://docs.cyverse.org/api/endpoints/misc/#retrieving-user-preferences) 

[`POST /secured/preferences`](https://docs.cyverse.org/api/endpoints/misc/#saving-user-preferences) 

[`DELETE /secured/sessions`](https://docs.cyverse.org/api/endpoints/misc/#removing-user-session-data) 

[`GET /secured/sessions`](https://docs.cyverse.org/api/endpoints/misc/#retrieving-user-session-data) 

[`POST /secured/sessions`](https://docs.cyverse.org/api/endpoints/misc/#saving-user-session-data) 

## send-notification

[`POST /send-notification.`](https://docs.cyverse.org/api/endpoints/notifications/#sending-an-arbitrary-notification) 

## uuid

[`GET /uuid`](https://docs.cyverse.org/api/endpoints/misc/#obtaining-identifiers) 

