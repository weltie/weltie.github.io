# Poetry-cmd

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
poetry install (代码会自动关联)

## repo config
   poetry config repositories.my-gitlab http://gitlab.my.com/api/v4/projects/36/packages/pypi --local
   poetry config http-basic.my-gitlab __token__ <gitlab-token> --local

## build and publish  
   poetry publish --repository yyxx-gitlab --build
