### Snapshots
Three more changes: from preparing to shipped, and related changes like addition of a tracking ID
- 38c516e8-b23a-493a-8a5c-bf7b2b9ea995
- aafb9fbd-56e1-4dcc-b6b2-a3fd91381bb6
- d1020671-7cdf-493c-b008-c48535415611

```SQL
SELECT
    *
FROM snapshot_orders
WHERE order_id IN (
    SELECT order_id
    FROM snapshot_orders
    WHERE dbt_valid_to > '2022-10-24'
)
ORDER BY order_id, dbt_valid_to DESC
;
```


### Modeling
##### How are our users moving through the product funnel
##### Which steps of the funnel have the largest drop off points

```SQL
USE DATABASE dev_db;
USE SCHEMA dbt_jmanahan;

SELECT
    SUM((page_view_count > 0)::INT) session_with_page_view_count
    , SUM((add_to_cart_count > 0)::INT) session_with_add_to_cart_count
    , SUM((checkout_count > 0)::INT) session_with_checkout_count
    , ROUND(
        session_with_add_to_cart_count / session_with_page_view_count
        , 4
      ) AS session_add_to_cart_rate
    , ROUND(
        session_with_checkout_count / session_with_page_view_count
        , 4
      ) AS session_checkout_rate
    , ROUND(
        session_with_checkout_count / session_with_add_to_cart_count
        , 4
      ) AS session_atc_to_checkout_rate
FROM tbl_session_metrics
;
```

Current