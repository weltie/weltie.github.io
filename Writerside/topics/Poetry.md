# Poetry

## installation

`pip install poetry`

## init

```Shell
poetry new poetry-demo

or

cd pre-existing-project
poetry init
```

## add

`poetry add xxx==xxx`

## install
`poetry install --all-extras`

## debug install
`poetry install (代码会自动关联)`

## repo config
```Shell
   poetry config repositories.my-gitlab http://gitlab.my.com/api/v4/projects/36/packages/pypi --local
   poetry config http-basic.my-gitlab __token__ <gitlab-token> --local
```

## build and publish  
```Bash
   poetry publish --repository my-gitlab --build
```

## Dockerfile build error
### before
```DOCKER
RUN pip install poetry &&  \
    poetry config virtualenvs.create false &&  \
    poetry install
```
### error
```plain text
  AttributeError

  'PythonSpec' object has no attribute 'free_threaded'

  at /usr/local/lib/python3.11/site-packages/virtualenv/discovery/py_info.py:339 in satisfies
      335│ 
      336│     @classmethod
      337│     def current(cls, app_data=None):
      338│         """
    → 339│         This locates the current host interpreter information. This might be different than what we run into in case
      340│         the host python has been upgraded from underneath us.
      341│         """"""  # noqa: D205
      342│         if cls._current is None:
      343│             cls._current = cls.from_exe(sys.executable, app_data, raise_on_error=True, resolve_to_host=False)

Cannot install pudb.
```
### after
```DOCKER
RUN pip install poetry==1.8.3\
    && poetry config virtualenvs.create false \
    && poetry install --no-interaction
```