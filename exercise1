-- Accounts created, deleted or merged per day

SELECT
  r.date AS Date,
  COALESCE(new.accounts_created, 0) AS Created,
  COALESCE(dlt.accounts_deleted, 0) AS Deleted,
  COALESCE(mrg.accounts_merged, 0) AS Merged,
  COALESCE(new.accounts_created, 0) - COALESCE(dlt.accounts_deleted, 0) - COALESCE(mrg.accounts_merged, 0) AS Net
FROM
  dsv1069.dates_rollup AS r
  LEFT JOIN (
    SELECT
      date(created_at) AS date,
      count(*) AS accounts_created
    FROM
      dsv1069.users
    GROUP BY
      date(created_at)
  ) new ON r.date = new.date
  LEFT JOIN (
    SELECT
      date(deleted_at) AS date,
      count(*) AS accounts_deleted
    FROM
      dsv1069.users
    WHERE
      deleted_at IS NOT NULL
    GROUP BY
      date(deleted_at)
  ) dlt ON new.date = dlt.date
  LEFT JOIN (
    SELECT
      date(merged_at) AS date,
      count(*) AS accounts_merged
    FROM
      dsv1069.users
    WHERE
      id <> parent_user_id
      AND merged_at IS NOT NULL
    GROUP BY
      date(merged_at)
  ) mrg ON mrg.date = dlt.date
ORDER BY
  r.date;
