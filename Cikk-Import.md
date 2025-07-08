# Yanac JSON Mapper, és cikkek betöltése.

## SQL lekérdezés futtatása:

```SQL
SELECT
	wp_posts.guid,
	wp_posts.post_title, 
	wp_posts.post_content,
	DATE(wp_posts.post_date) AS post_date,
    wp_users.user_nicename,
    wp_terms.name AS tags,
    '#2d63b9' AS color
FROM wp_posts 
INNER JOIN wp_term_relationships 
	ON wp_posts.ID = wp_term_relationships.object_id
INNER JOIN wp_users
	ON wp_posts.post_author = wp_users.ID
INNER JOIN wp_terms 
	ON wp_term_relationships.term_taxonomy_id = wp_terms.term_id 
WHERE wp_posts.post_status = 'publish'
  AND wp_term_relationships.term_taxonomy_id NOT IN (1, 11, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33);

```

! Eredmény elmentése fájlba.

## OCTI Konfiguráció

Report types megadása `Settings/Taxonomies/Vocabularies/report_types_ov`

## JSON Mapper létrehozása

`YANAC-Cikk-JSON_MAPPER_Report.json` néven elmentve

```
{"openCTI_version":"6.7.3","type":"jsonMapper","configuration":{"name":"YANAC Report","variables":[],"representations":[{"id":"8cd7199d-bc34-4fce-bae4-508e5a4c6816","type":"entity","target":{"entity_type":"Report","path":"$..data"},"identifier":"$..guid","attributes":[{"mode":"simple","key":"confidence","default_values":["60"],"attr_path":null},{"mode":"simple","key":"name","default_values":null,"attr_path":{"path":"$..post_title","configuration":null,"independent":null}},{"mode":"simple","key":"description","default_values":null,"attr_path":{"path":"$..post_content","configuration":null,"independent":null}},{"mode":"simple","key":"published","default_values":null,"attr_path":{"path":"$..post_date","configuration":{"pattern_date":"YYY-MM-SS","separator":null,"timezone":null},"independent":null}},{"mode":"base","key":"objectLabel","default_values":null,"based_on":{"identifier":"$..guid","representations":["28ce58f4-472e-41df-a861-1d4d8bf6dde8"]}}]},{"id":"67cab1a2-90ff-4b9b-ae0c-20b8e5d3b78c","type":"entity","target":{"entity_type":"Individual","path":"$..data"},"identifier":"$..guid","attributes":[{"mode":"simple","key":"name","default_values":null,"attr_path":{"path":"$..user_nicename","configuration":null,"independent":null}}]},{"id":"28ce58f4-472e-41df-a861-1d4d8bf6dde8","type":"entity","target":{"entity_type":"Label","path":"$..data"},"identifier":"$..guid","attributes":[{"mode":"simple","key":"value","default_values":null,"attr_path":{"path":"$..tags","configuration":null,"independent":null}},{"mode":"simple","key":"color","default_values":null,"attr_path":{"path":"$..color","configuration":null,"independent":null}}]}]}}
```

## JSON Ingestion

A JSON fájlhoz tartozó link megadása, a feed beállítása.
https://raw.githubusercontent.com/Lensver65/YASOC/refs/heads/main/wp_posts%20(1).json

Aztán mehet is.

Kivéve, ha túl nagy a fájl, mert akkor azt darabolni kell. Vagy máshová feltölteni...