# Scheduler

It's pretty common at some point during a site build to find you need to schedule the odd task. This would traditionally mean setting a CRON job for each. To avoid the confusion of multiple CRON jobs and their different settings we can rely on the Scheduler, which requires just one CRON for your entire site. Its a stripped down version of [Laravels Scheduler]( http://laravel.com/docs/5.0/artisan#scheduling-artisan-commands).

### Set Up

Lets step the one CRON job to get the scheduler active:

```
* * * * * php /path/to/site/suzie/bootstrap/scheduler.php 1>> /dev/null 2>&1
```  
To clarify if the path to my site was:

```
/var/www/my-new-wordpress-site/
```
Then the CRON would be:
```
* * * * * php /var/www/my-new-wordpress-site/suzie/bootstrap/scheduler.php 1>> /dev/null 2>&1
```

### The Schedule

You should define your events in  `public/schedule.php` using a closure:

``` php
$schedule->call(function()
{
  //do something
})->everyFiveMinutes();
```

Below is a list of the available time settings:

```
Cron expression.
$schedule->call()-cron($expression)

Hourly.
$schedule->call()->hourly()

Daily.
$schedule->call()->daily()

Given time.
$schedule->call()->at()

Daily at a given time (10:00, 19:30, etc).
$schedule->call()->dailyAt($time)

Twice daily.
$schedule->call()->twiceDaily()

Only on weekdays.
$schedule->call()->weekdays()

Only on Mondays.
$schedule->call()->mondays()

Only on Tuesdays.
$schedule->call()->tuesdays()

Only on Wednesdays.
$schedule->call()->wednesdays()

Only on Thursdays.
$schedule->call()->thursdays()

Only on Fridays.
$schedule->call()->fridays()

Only on Saturdays.
$schedule->call()->saturdays()

Only on Sundays.
$schedule->call()->sundays()

Weekly.
$schedule->call()->weekly()

Weekly on a given day and time.
$schedule->call()->weeklyOn($day, $time = '0:0')

Monthly.
$schedule->call()->monthly()

Yearly.
$schedule->call()->yearly()

Every five minutes.
$schedule->call()->everyFiveMinutes()

Every ten minutes.
$schedule->call()->everyTenMinutes()

Every thirty minutes.
$schedule->call()->everyThirtyMinutes()
```
