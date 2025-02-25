# Standardized Logging Configuration

This file documents a standardized logging configuration using Python's built-in `logging` module. Use this configuration to ensure consistency across your projects.

## Logging Configuration Code

```python
import logging
import logging.config

# Define the logging configuration in a dictionary
LOGGING_CONFIG = {
    'version': 1,
    'disable_existing_loggers': False,
    'formatters': {
        'standard': {
            'format': '%(asctime)s [%(levelname)s] %(name)s: %(message)s'
        },
    },
    'handlers': {
        'console': {
            'class': 'logging.StreamHandler',
            'formatter': 'standard',
            'level': 'DEBUG',
            'stream': 'ext://sys.stdout'
        },
        'file': {
            'class': 'logging.handlers.RotatingFileHandler',
            'formatter': 'standard',
            'level': 'INFO',
            'filename': 'app.log',
            'maxBytes': 10485760,  # 10 MB
            'backupCount': 3,
            'encoding': 'utf8'
        },
    },
    'root': {
        'handlers': ['console', 'file'],
        'level': 'DEBUG',
    },
    'loggers': {
        'myapp': {
            'handlers': ['console', 'file'],
            'level': 'DEBUG',
            'propagate': False
        },
    }
}

def setup_logging():
    """Apply the logging configuration."""
    logging.config.dictConfig(LOGGING_CONFIG)
    logging.getLogger(__name__).debug("Logging is configured.")

# Run setup at the start of your application
setup_logging()

# Example usage
logger = logging.getLogger('myapp')
logger.info("This is an info message.")
logger.debug("This is a debug message.")
logger.error("This is an error message.")

```

## How to Use:

- Save this code in a file (or module) named, for example, `logging_config.py`.
- Import and call `setup_logging()` at the beginning of your application to initialize the logging system.

```python
from logging_config import setup_logging

setup_logging()

```

This setup outputs logs both to the console (DEBUG and above) and to a rotating log file (`app.log`) for INFO level and above.