# Celery

------------------

## Getting Started

```
pip install celery==5.2.7
```

- Choosing a Broker
    + RabbitMQ
    + Redis

- App

```python
from celery import Celery

app = Celery('tasks', broker='pyamqp://guest@localhost//')


@app.task
def add(x, y):
    return x + y
```

- Run

```
celery -A tasks worker --loglevel=INFO
```

## Task

```python
from celery import shared_task


@shared_task
def add(x, y):
    return x + y


@app.task(bind=True)
def add(self, x, y):
    logger.info(self.request.id)
```

- Ignore results you don’t want
- Avoid launching synchronous subtasks

```python
# Bad
@app.task
def update_page_info(url):
    page = fetch_page.delay(url).get()
    info = parse_page.delay(page).get()
    store_page_info.delay(url, info)


@app.task
def fetch_page(url):
    return myhttplib.get(url)


@app.task
def parse_page(page):
    return myparser.parse_document(page)


@app.task
def store_page_info(url, info):
    return PageInfo.objects.create(url, info)


# Good
def update_page_info(url):
    # fetch_page -> parse_page -> store_page
    chain = fetch_page.s(url) | parse_page.s() | store_page_info.s(url)
    chain()


@app.task()
def fetch_page(url):
    return myhttplib.get(url)


@app.task()
def parse_page(page):
    return myparser.parse_document(page)


@app.task(ignore_result=True)
def store_page_info(info, url):
    PageInfo.objects.create(url=url, info=info)
```

- Database transactions

```python
article = Article.objects.create()
expand_abbreviations.delay(article.pk)

# The solution is to use the on_commit callback to launch 
# your Celery task once all transactions have been committed successfully.
article = Article.objects.create()
transaction.on_commit(lambda: expand_abbreviations.delay(article.pk))
```

- Call Task

```python
apply_async(args[, kwargs[, …]])
delay(*args, **kwargs)
calling(__call__)  # will not be executed by a worker
```

- Serializers
    + json: strings, Unicode, floats, Boolean, dictionaries, and lists. Decimals and dates are notably missing.
    + pickle: support of all built-in Python data types (except class instances)
    + yaml: has many of the same characteristics as json, except that it natively supports more data types (including
      dates, recursive references, etc.). However, the Python libraries for YAML are a good bit slower than the
      libraries for JSON.
    + msgpack – msgpack is a binary serialization format that’s closer to JSON in features. The format compresses
      better, so is a faster to parse and encode compared to JSON.
- Concurrency By default multiprocessing is used to perform concurrent execution of tasks, but you can also use
  Eventlet. The number of worker processes/threads can be changed using the --concurrency argument and defaults to the
  number of CPUs available on the machine.

## Periodic Tasks

```python
from celery.schedules import crontab

app.conf.beat_schedule = {
    # Executes every Monday morning at 7:30 a.m.
    'add-every-monday-morning': {
        'task': 'tasks.add',
        'schedule': crontab(hour=7, minute=30, day_of_week=1),
        'args': (16, 16),
    },
}

```

```
celery -A proj beat
```

## Routing Tasks

```python

app.conf.task_default_queue = 'default'
app.conf.task_queues = (
    Queue('default', routing_key='task.#'),
    Queue('feed_tasks', routing_key='feed.#'),
)
app.conf.task_default_exchange = 'tasks'
app.conf.task_default_exchange_type = 'topic'
app.conf.task_default_routing_key = 'task.default'
```

- Create task

```python
@shared_task(queue='feed_tasks')
def ping_task():
    return 'pong'
```

- Run

``` 
celery -A proj worker -Q feed_tasks 
```

## Signals

- Task Signals
    + before_task_publish
    + after_task_publish
    + task_prerun
    + task_postrun
    + task_retry
    + task_success
    + task_failure
    + task_internal_error
    + task_received
    + task_revoked
    + task_unknown
    + task_rejected

- App Signals
    + import_modules
    + Worker Signals
    + celeryd_after_setup
    + celeryd_init
    + worker_init
    + worker_before_create_process
    + worker_ready
    + heartbeat_sent
    + worker_shutting_down
    + worker_process_init
    + worker_process_shutdown
    + worker_shutdown

- Beat Signals
    + beat_init
    + beat_embedded_init
    + Eventlet Signals
    + eventlet_pool_started
    + eventlet_pool_preshutdown
    + eventlet_pool_postshutdown
    + eventlet_pool_apply

- Logging Signals
    + setup_logging
    + after_setup_logger
    + after_setup_task_logger

- Command signals
    + user_preload_options
    + Deprecated Signals
    + task_sent

## Ensuring a task is only executed one at a time

```python
import time
from celery import task
from celery.utils.log import get_task_logger
from contextlib import contextmanager
from django.core.cache import cache
from hashlib import md5
from djangofeeds.models import Feed

logger = get_task_logger(__name__)

LOCK_EXPIRE = 60 * 10  # Lock expires in 10 minutes


@contextmanager
def memcache_lock(lock_id, oid):
    timeout_at = time.monotonic() + LOCK_EXPIRE - 3
    # cache.add fails if the key already exists
    status = cache.add(lock_id, oid, LOCK_EXPIRE)
    try:
        yield status
    finally:
        # memcache delete is very slow, but we have to use it to take
        # advantage of using add() for atomic locking
        if time.monotonic() < timeout_at and status:
            # don't release the lock if we exceeded the timeout
            # to lessen the chance of releasing an expired lock
            # owned by someone else
            # also don't release the lock if we didn't acquire it
            cache.delete(lock_id)


@task(bind=True)
def import_feed(self, feed_url):
    # The cache key consists of the task name and the MD5 digest
    # of the feed URL.
    feed_url_hexdigest = md5(feed_url).hexdigest()
    lock_id = '{0}-lock-{1}'.format(self.name, feed_url_hexdigest)
    logger.debug('Importing feed: %s', feed_url)
    with memcache_lock(lock_id, self.app.oid) as acquired:
        if acquired:
            return Feed.objects.import_feed(feed_url).url
    logger.debug(
        'Feed %s is already being imported by another worker', feed_url)
```

## Config

- accept_content
- result_accept_content
- worker_concurrency
- task_ignore_result
