### Snapshots
[Must wait until ~Monday]


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