# cumulus-publish-cnm

This is the Python code for the lambda `cumulus-publish-cnm`. It takes in a list of cnm's (Cloud Notification Mechanism) and publishes each one to the specified ingestion SNS (provider-input-sns)

## Getting Started

### Dependencies

* [Poetry](https://python-poetry.org/docs/#installing-with-the-official-installer)
* Optional: [pyenv](https://github.com/pyenv/pyenv) and [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv)

### Installing

* Option 1: Use `pyenv` to create a virtual environment and activate it

        $ pyenv virtualenv 3.11.9 cumulus-publish-cnm
        $ pyenv activate cumulus-publish-cnm
        $ poetry shell

* Option 2: Use Poetry to create and activate a virtual environment

        $ poetry shell

* Install dependencies

        $ poetry install

### Build

        $ ./build.sh

### Run Tests

        $ poetry run pytest tests

### Manual Build

This command creates the `dist/` folder:

        $ poetry build

This command downloads the dependency files from the just created .whl file, along with the lambda_handler function in `cumulus_publish_cnm/cumulus_publish_cnm.py`, and places them in the `package` folder:

        $ poetry run pip install --upgrade -t package dist/*.whl

Note that **boto3** and **moto** are not being pulled in. I've specifically excluded them via having them as `tool.poetry.dev-dependencies` only (via the `pyproject.toml` file)

The last command used is:

        $ cd package ; zip -r ../artifact.zip . -x '*.pyc'

Which zips and creates the `artifact.zip` file, containing all files found in `package/` excluding .pyc files


### Running the Lambda

Upload `artifact.zip` to any location you plan to use it

* Upload directly as a lambda with `aws lambda update-function-code`
* Upload to your AWS S3 `lambdas/` folder so that your Cumulus Terraform Build can use it

NOTE: This AWS Lambda Function is intended to be used as a cumulus task and this requires `sns_endpoint` from the `task_config`:

    "task_config": {
        "sns_endpoint": "{$.meta.ingest_workflow_sns}"
    }

## Help

`This <https://chariotsolutions.com/blog/post/building-lambdas-with-poetry/>`_ page contains some good info on the overall "building a zip with poetry that's compatible with AWS Lambda".

## Version History

See the [CHANGELOG](CHANGELOG.md)

## License

TBD

## Acknowledgments

Forked from PODAAC's [cumulus-publish-cnm](https://github.com/podaac/cumulus-publish-cnm) GitHub repo
