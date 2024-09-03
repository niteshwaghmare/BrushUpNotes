# Python Logger

In Python, the `logging` module provides a flexible framework for emitting log messages from Python programs. The `Logger` class in the `logging` module is a central component of this framework.

## Logging Levels

1. **DEBUG:**
   - Use `logger.debug()` for detailed information, typically useful for debugging purposes.
   - Example: Logging variable values, detailed function calls, etc.

    ```python
    logger.debug('This is a debug message. Variable value: %s', some_variable)
    ```

2. **INFO:**
   - Use `logger.info()` for general information about the program's execution.
   - Example: Indicating the start and end of significant operations.

    ```python
    logger.info('Processing started...')
    ```

3. **WARNING:**
   - Use `logger.warning()` to indicate that something unexpected happened, but the program can still continue.
   - Example: Non-critical issues that might affect the program's behavior.

    ```python
    logger.warning('This is a warning message. Proceeding with caution...')
    ```

4. **ERROR:**
   - Use `logger.error()` to indicate a more severe problem that prevented the program from performing a certain function.
   - Example: Unexpected errors that need attention.

    ```python
    try:
        # Some code that might raise an exception
    except Exception as e:
        logger.error('An error occurred: %s', str(e), exc_info=True)
    ```

5. **CRITICAL:**
   - Use `logger.critical()` for very severe errors, indicating that the program may be unable to continue running.
   - Example: Critical errors that require immediate attention.

    ```python
    logger.critical('This is a critical error. The program cannot continue.')
    ```

The `exc_info=True` parameter in the logging statements can be used to include exception information in the log output.

## Basic configuration example

Certainly! Setting up a good logging configuration for a Python project involves configuring loggers, handlers, and formatters. Here's an example configuration using best practices:

```python
import logging
from logging.handlers import RotatingFileHandler
import sys

def setup_logging():
    # Create a formatter with a detailed log format
    formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')

    # Create a handler for console output (stream handler)
    console_handler = logging.StreamHandler(sys.stdout)
    console_handler.setFormatter(formatter)
    
    # Create a handler for file output (Basic one)
    # file_handler = logging.FileHandler('app.log')
    # file_handler.setFormatter(formatter)

    # Create a rotating file handler with log rotation (max 1 MB, keep 3 backups)
    file_handler = RotatingFileHandler('app.log', maxBytes=1e6, backupCount=3)
    file_handler.setFormatter(formatter)
    
    # Create a logger and set its level
    logger = logging.getLogger('your_project_name')
    logger.setLevel(logging.DEBUG)  # Set for the development
    # logger.setLevel(logging.INFO) # Set for the production 


    # Add the handlers to the logger
    logger.addHandler(console_handler)
    logger.addHandler(file_handler)

    return logger

# Example usage:
if __name__ == "__main__":
    # Use the logger throughout your application
    logger = setup_logging()

    try:
        logger.info('Application started')

        # Your application code here...

        logger.info('Application completed successfully')

    except Exception as e:
        # Log any unhandled exceptions
        logger.exception('An unhandled exception occurred: %s', str(e))

        # Optionally: Raise the exception again to halt the program
        raise
```

Explanation:

1. **Formatter:**
   - The `Formatter` class is used to specify the format of log records. In this example, it includes the timestamp, logger name, log level, and the log message.
2. **Handlers:**
   - Two handlers are created: `console_handler` for streaming logs to the console and `file_handler` for writing logs to a file named 'app.log'.
   - Each handler is configured with the formatter created earlier.
3. **RotationHandler**
   - The `RotatingFileHandler` is used for log rotation. It rotates the log file when it reaches 1 MB (`maxBytes=1e6`) and keeps up to 3 backup copies of old log files (`backupCount=3`).
4. **Logger:**
   - The `getLogger` method is used to create a logger instance with the specified name ('your_project_name' in this case).
   - The logger's level is set to `logging.DEBUG`, meaning it will capture messages of all levels.
5. **Logger Usage:**
   - Throughout your application, use the logger for logging messages at various levels.
   - In the example usage section, there's a try-except block to log any unhandled exceptions with the `exception` method, which includes exception information in the log.

This is a basic setup that you can customize according to your project's needs. You might adjust the log levels, add more handlers (e.g., for email or network logging), or use a configuration file for more flexibility.

## Production vs Development Logger

Typically, in production, you might set the logging level to `INFO` or higher. The levels, in increasing order of severity, are `DEBUG`, `INFO`, `WARNING`, `ERROR`, and `CRITICAL`. Setting the level to `INFO` means that only messages of `INFO` level and above will be captured, filtering out `DEBUG` messages.

## basicConfig

The `basicConfig` method in the `logging` module is a convenient way to configure the root logger and set up a basic logging configuration. It's a simple and quick way to configure logging for simple applications or scripts. `basicConfig` is typically called only once at the beginning of your program.

Here is a simple example:

```python
import logging

# Configuring the root logger using basicConfig
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')

# Now you can use the root logger throughout your application
logging.info('This is an informational message')
```

The `basicConfig` function can take several parameters to configure the root logger, such as:

- `level`: Sets the root logger level.
- `filename`: Specifies a file to write logs to (if not specified, logs will go to the console).
- `filemode`: The mode to open the file in (e.g., 'w' for write, 'a' for append).
- `format`: The format of log messages.

**When it's good to use `basicConfig`:**

1. **Simple scripts or small applications:** If your application is small and doesn't require a complex logging setup, `basicConfig` can be a quick and easy way to get logging up and running.

2. **Quick debugging:** For debugging purposes during development, you might use `basicConfig` to quickly set up logging to the console and get insights into the program's behavior.

3. **Rapid prototyping:** In situations where you want a minimal logging setup for rapid prototyping or testing, `basicConfig` can be handy.

**Example:**

```python
import logging

# Configure the root logger to log to the console with INFO level
logging.basicConfig(level=logging.INFO)

# Now you can use the root logger throughout your application
logging.info('This is an informational message')
```

Keep in mind that for larger or more complex applications, it's often better to set up a more sophisticated logging configuration using `Logger`, `Handler`, and `Formatter` objects, especially when dealing with multiple modules or components. The `basicConfig` method is just a quick and simple way to get started with logging.

## Logging in Django

```python
# settings.py

import os
import logging
from logging.handlers import RotatingFileHandler

# Set the base directory of your Django project
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))

LOGGING = {
    'version': 1,  
	  # django uses some of its own loggers for internal operations. In case you want disable them just replace the False above with true.
    'disable_existing_loggers': False,

    'formatters': {
        "verbose": {
            "format": "{levelname} {asctime} {module} {process:d} {thread:d} {message}",
            "style": "{",
        },
        "simple": {
            "format": "{levelname} {message}",
            "style": "{",
        },
    },

    # A handler for WARNING. It is basically writing the WARNING messages into a file called WARNING.log
    'handlers': {
        "file": {
            'level': 'DEBUG',
            'class': 'logging.handlers.RotatingFileHandler',
            'filename': os.path.join(BASE_DIR, 'logs', 'django.log'),
            'formatter': 'verbose',
            'backupCount': 5,  # keep at most 5 log files
            'maxBytes': 10 * 1024 * 1024,  # 10 MB
        },
        "console": {
            "class": "logging.StreamHandler",
            "formatter": "verbose",
        }
    },
    # A logger for WARNING which has a handler called 'file'. A logger can have multiple handler
    'loggers': {
        '': {
            'handlers': ['file', 'console'],
            # notice how file variable is called in handler which has been defined above
            'level': 'DEBUG',
            'propagate': True,
        },
    },
}

```

```python
# views.py or any other Django file

import logging

# Get the logger for the current module or use a custom logger name
logger = logging.getLogger(__name__)

def my_view(request):
    logger.info('This is an informational message in a Django view')
    # Your view logic here...
```

## Best practices 

Certainly! Here are some best practices for logging in Python applications:

1. **Use Descriptive Logger Names:**
   - Choose meaningful names for your loggers that reflect the components or modules they belong to. This helps in identifying the source of log messages.

   ```python
   logger = logging.getLogger('my_module')
   ```

2. **Configure Logging at the Beginning:**
   - Set up your logging configuration early in your application, preferably at the beginning. This ensures that the logging behavior is consistent throughout your code.

   ```python
   import logging
   
   logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(name)s - %(levelname)s - %(message)s')
   ```

3. **Use Different Log Levels:**
   - Use appropriate log levels (DEBUG, INFO, WARNING, ERROR, CRITICAL) based on the severity of the message. Avoid overly verbose logging in production.

   ```python
   logger.debug('This is a debug message')
   logger.info('This is an informational message')
   logger.warning('This is a warning message')
   logger.error('This is an error message')
   logger.critical('This is a critical message')
   ```

4. **Include Relevant Information:**
   - Include relevant information in log messages, such as variable values, context, and exception details. This aids in troubleshooting.

   ```python
   logger.error('An error occurred: %s', str(e), exc_info=True)
   ```

5. **Use Logging Format:**
   - Define a clear and consistent logging format using `Formatter` to include relevant information like timestamp, logger name, log level, and the log message.

   ```python
   formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
   ```

6. **Avoid Using Print for Logging:**
   - While `print` statements are handy for debugging, use the logging module for structured and configurable logging. It provides better control and flexibility.

7. **Avoid Hardcoding Log File Paths:**
   - If logging to files, avoid hardcoding file paths. Instead, use `os.path.join` to create paths dynamically, and ensure the directories exist.

   ```python
   import os
   
   log_file_path = os.path.join('logs', 'app.log')
   ```

8. **Use Logger Propagation Sparingly:**
   - Logger propagation can be useful, but use it judiciously. In most cases, it's better to configure handlers for individual loggers rather than relying on propagation.

   ```python
   logger.propagate = False
   ```

9. **Handle Exceptions:**
   - Use `try-except` blocks to catch and log exceptions. This ensures that unexpected errors are captured in the logs.

   ```python
   try:
       # Your code here...
   except Exception as e:
       logger.exception('An error occurred: %s', str(e))
   ```

10. **Configure Different Handlers:**
   - Configure different handlers for different outputs (console, file, network, etc.). This allows you to control where log messages are sent.

   ```python
   console_handler = logging.StreamHandler()
   file_handler = logging.FileHandler('app.log')
   ```

11. **Rotate Log Files:**
   - If logging to files, consider using log rotation to manage log file size. The `RotatingFileHandler` or `TimedRotatingFileHandler` can be used for this purpose.

   ```python
   from logging.handlers import RotatingFileHandler

   file_handler = RotatingFileHandler('app.log', maxBytes=1e6, backupCount=3)
   ```

12. **Consider Log Level in Production:**
   - In production, set the logging level to a higher level than `DEBUG` (e.g., `INFO` or `WARNING`) to avoid excessive log verbosity.

   ```python
   logging.basicConfig(level=logging.INFO)
   ```

13. **Use Logging in Django Applications:**
   - In Django, configure logging in the `settings.py` file. Utilize loggers for different components, and consider using the Django `logging` settings.

   ```python
   # settings.py

   LOGGING = {
       # Configuration here...
   }
   ```

These practices contribute to a well-organized and effective logging strategy in your Python applications. Adjust them based on the specific requirements and size of your project.