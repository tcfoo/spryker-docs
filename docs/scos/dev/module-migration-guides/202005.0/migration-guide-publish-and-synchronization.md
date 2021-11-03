---
title: Migration Guide - Publish and Synchronization
description: Use the guide to learn how to update the Publish and Synchronization module to a newer version.
last_updated: Apr 3, 2020
template: module-migration-guide-template
originalLink: https://documentation.spryker.com/v5/docs/mg-pub-and-sync
originalArticleId: dc7cce9a-e004-48a0-ac2d-2f3dd3269c98
redirect_from:
  - /v5/docs/mg-pub-and-sync
  - /v5/docs/en/mg-pub-and-sync
---

## Upgrading from Version 0.* to Version 1.*

### Version 1 of the Storage and the Search modules

#### Table Indexs
In this version, Indexes were added to Storage and Search tables, this will increase the performance of Listeners and workers.

#### Store and Redis Keys
Currently, Spryker supports multi-store and this should be adopted for the rest of the module. Some of the Storage and Search modules are not store aware, so for this reason the store column might be removed and this will impact the Redis key. Structure didn't change. For those modules which do not belong to any store, we need to add a new property to `schema.xml` files

```xml
<behavior name="synchronization">
	<parameter name="queue_pool" value="synchronizationPool" />
</behavior>
```
For wiring the stores and sync queues you should configure it in `store.php`

```php
$stores['DE']['queuePools']['synchronizationPool'] = [
	'AT-connection',
	'DE-connection'
];
```
#### Module Layers
In Previous version the listener plugins has been extended from Abstract plugin classes and now this has changed due to obey the spryker architecture and moved into business layer and open APIs from Facade classes.

### Version 1 of the EventBehaivor Modules

#### PropelPlugin
The type of primary key of `spy_event_behavior_entity_change` table (`id_event_behavior_entity_change`) changed from `INTEGER` to `BIGINT`

### Version 1 of the SynchronizationBehaivor modules

#### PropelPlugin
A new property `queue_pool` has been added to `SynchronizationBehavior`, this allow to send the data to specific queue connection pool.
