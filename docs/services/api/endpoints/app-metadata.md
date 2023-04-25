**Jump to:**

* [Application Metadata Endpoints](#application-metadata-endpoints)

* [Adding Categories](#adding-categories)

* [Searching for Categories by Name](#searching-for-categories-by-name)
    
* [Deleting a Category by ID](#deleting-a-category-by-id)

* [Updating an App Category](#updating-an-app-category)

* [Managing App AVU Metadata](#managing-app-avu-metadata)

* [Listing App Categories](#listing-app-categories)

* [Listing Workspaces](#listing-workspaces)

* [Deleting Workspaces](#deleting-workspaces)

# Application Metadata Endpoints

Note that secured endpoints in Terrain and apps are a little different from each other.
Please see [Terrain Vs. Apps](terrain-v-apps.md) for more information.

## Adding Categories

* [POST /admin/apps/categories/{system-id}](https://de.cyverse.org/terrain/docs/index.html#!/default/post_terrain_admin_apps_categories_system_id)

This endpoint is a passthrough to the apps endpoint using the same path.
Please see the apps service documentation for more information.

## Searching for Categories by Name

* [GET /admin/apps/categories/search](https://de.cyverse.org/terrain/docs/index.html#!/default/get_terrain_admin_apps_categories_search)

This endpoint is a passthrough to the apps endpoint using the same path.
Please see the apps service documentation for more information.

## Deleting a Category by ID

* [DELETE /admin/apps/categories/{system-id}/{category-id}](https://de.cyverse.org/terrain/docs/index.html#!/default/delete_terrain_admin_apps_categories_system_id_category_id)

This endpoint is a passthrough to the apps endpoint using the same path.
Please see the apps service documentation for more information.

## Updating an App Category

* [PATCH /admin/apps/categories/{system-id}/{category-id}](https://de.cyverse.org/terrain/docs/index.html#!/default/patch_terrain_admin_apps_categories_system_id_category_id)

This endpoint is a passthrough to the apps endpoint using the same path.
Please see the apps service documentation for more information.

## Managing App AVU Metadata

* [GET /admin/apps/{app-id}/metadata](https://de.cyverse.org/terrain/docs/index.html#!/default/get_terrain_admin_apps_app_id_metadata)
* [POST /admin/apps/{app-id}/metadata](https://de.cyverse.org/terrain/docs/index.html#!/default/post_terrain_admin_apps_app_id_metadata)
* [PUT /admin/apps/{app-id}/metadata](https://de.cyverse.org/terrain/docs/index.html#!/default/put_terrain_admin_apps_app_id_metadata)

These endpoints are passthroughs to the apps endpoints using the same paths.
Please see the corresponding service documentation for more information.

## Listing App Categories

* [GET /admin/apps/categories](https://de.cyverse.org/terrain/docs/index.html#!/default/get_terrain_admin_apps_categories)

Delegates to apps: `GET /admin/apps/categories`

These endpoints are passthroughs to the apps endpoints using the same path.
Please see the apps service documentation for more information.

## Listing Workspaces

* [GET /admin/workspaces](https://de.cyverse.org/terrain/docs/index.html#!/default/get_terrain_admin_workspaces)

This service is a passthrough to the apps endpoint using the same path.
Please see the apps service documentation for more details.

## Deleting Workspaces

* [DELETE /admin/workspaces](https://de.cyverse.org/terrain/docs/index.html#!/default/delete_terrain_admin_workspaces)

This service is a passthrough to the apps endpoint using the same path.
Please see the apps service documentation for more details.
