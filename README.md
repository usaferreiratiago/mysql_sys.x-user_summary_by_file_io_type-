# mysql_sys.x-user_summary_by_file_io_type-

SELECT 
    IF((`performance_schema`.`events_waits_summary_by_user_by_event_name`.`USER` IS NULL),
        'background',
        `performance_schema`.`events_waits_summary_by_user_by_event_name`.`USER`) AS `user`,
    `performance_schema`.`events_waits_summary_by_user_by_event_name`.`EVENT_NAME` AS `event_name`,
    `performance_schema`.`events_waits_summary_by_user_by_event_name`.`COUNT_STAR` AS `total`,
    `performance_schema`.`events_waits_summary_by_user_by_event_name`.`SUM_TIMER_WAIT` AS `latency`,
    `performance_schema`.`events_waits_summary_by_user_by_event_name`.`MAX_TIMER_WAIT` AS `max_latency`
FROM
    `performance_schema`.`events_waits_summary_by_user_by_event_name`
WHERE
    ((`performance_schema`.`events_waits_summary_by_user_by_event_name`.`EVENT_NAME` LIKE 'wait/io/file%')
        AND (`performance_schema`.`events_waits_summary_by_user_by_event_name`.`COUNT_STAR` > 0))
ORDER BY IF((`performance_schema`.`events_waits_summary_by_user_by_event_name`.`USER` IS NULL),
    'background',
    `performance_schema`.`events_waits_summary_by_user_by_event_name`.`USER`) , `performance_schema`.`events_waits_summary_by_user_by_event_name`.`SUM_TIMER_WAIT` DESC
