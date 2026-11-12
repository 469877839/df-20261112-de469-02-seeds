# Local data-engineering evaluation baseline

Upstream: https://github.com/cloudevents/sdk-python/commit/adfe3dfe3a1f199fe19b6d735d62121dd7eba3c0
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
.venv/bin/python -B -m pytest -q -o addopts= -p no:cacheprovider tests/test_core/test_bindings/test_http.py tests/test_core/test_format/test_json.py tests/test_core/test_v1/test_event.py tests/test_v1_compat/test_http_conversions.py
```

The selected tests exercise local fixtures or in-process clients. Restore by making a fresh checkout.
