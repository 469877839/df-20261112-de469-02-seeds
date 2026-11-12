# Local data-engineering evaluation baseline

Upstream: https://github.com/django-import-export/django-import-export/commit/0d550a0bd6de44620ff19f40fb7bcde0c3a37ea3
Original license and attribution files remain. The code and test fixtures come from that commit.
Upstream CI workflows are omitted. This snapshot adds the environment instructions and dependency lock.


Tested on Linux with CPython 3.13.15. Start from a fresh checkout of this commit:

```sh
uv venv --python 3.13.15 .venv
export PATH="$PWD/.venv/bin:$PATH"
export PYTHONUTF8=1 PYTHONDONTWRITEBYTECODE=1 PYTEST_DISABLE_PLUGIN_AUTOLOAD=1 AWS_EC2_METADATA_DISABLED=true
mkdir -p .venv/tmp
export TMPDIR="$PWD/.venv/tmp"
uv pip install --python .venv/bin/python -r requirements-domain.lock
uv pip install --python .venv/bin/python --no-build-isolation --no-deps -e .
.venv/bin/python -B tests/manage.py test core.tests.test_resources.test_modelresource.test_data_import core.tests.test_resources.test_bulk_operations core.tests.test_resources.test_modelresource.test_error_handling core.tests.test_resources.test_natural_foreign_key --verbosity=1
```

The selected tests exercise local fixtures or in-process clients. Restore by making a fresh checkout.
