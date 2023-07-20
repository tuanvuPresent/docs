# Logging

---
### Các loại log

---
  + DEBUG:	  Thông tin chi tiết, thường là thông tin để tìm lỗi
  + INFO:	  Thông báo thông thường, các thông tin in ra khi chương trình chạy theo đúng kịch bản
  + WARNING:  Thông báo khi nghi vấn bất thường hoặc lỗi có thể xảy ra, tuy nhiên chương trình vẫn có thể hoạt động
  + ERROR: 	  Lỗi, chương trình có thể không hoạt động được một số chức năng hoặc nhiệm vụ nào đó, thường thì nên dùng ghi bắt được Exception
  + CRITICAL: Lỗi, chương trình gặp lỗi nghiêm trọng không thể giải quyết được và bắt buộc phải dừng lại

### VD:

---

```python
import logging

from pythonjsonlogger import jsonlogger

logger = logging.getLogger(__name__)

logger.setLevel(logging.DEBUG)
logHandler = logging.StreamHandler()
logHandler.setFormatter(jsonlogger.JsonFormatter())
logger.addHandler(logHandler)


logger.info('', extra={'info': 'debugThis message should go to the log file'})
logger.debug('', extra={'debug': 'debugThis message should go to the log file'})
logger.error('', extra={'error': 'debugThis message should go to the log file'})
logger.warning('', extra={'warning': 'debugThis message should go to the log file'})
```

### Custom logging

---

```python
import logging

from pythonjsonlogger import jsonlogger


class CustomJsonFormatter(jsonlogger.JsonFormatter):
    def add_fields(self, log_record, record, message_dict):
        log_record['level'] = record.levelname
        log_record['name'] = record.name
        super(CustomJsonFormatter, self).add_fields(log_record, record, message_dict)


def get_logger(name):
    log_handler = logging.StreamHandler()
    log_handler.setFormatter(CustomJsonFormatter())

    _logger = logging.getLogger(name)
    _logger.setLevel(logging.DEBUG)
    _logger.addHandler(log_handler)
    return _logger

debug_logger = get_logger('debug')
debug_logger.info("info", extra={"special": "value", "run": 12})
```
- Hoặc có thế dùng dictConfig 
```python
import logging.config

logging.config.dictConfig({
    'version': 1,
    'disable_existing_loggers': False,
    'formatters': {
        'json': {
            '()': 'pythonjsonlogger.jsonlogger.JsonFormatter',
        },
    },
    'handlers': {
        'console': {
            'class': 'logging.StreamHandler',
            'formatter': 'json',
        },
    },
    'loggers': {
        '': {
            'handlers': ['console'],
            'level': 'DEBUG',
        },
    },
})
logger = logging.getLogger('access_log')
```
