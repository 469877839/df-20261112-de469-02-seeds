# Local data-engineering evaluation baseline

Upstream: https://github.com/remix/partridge/commit/7b3d2c70281ef4f5437c3e48d786569c1702ebd8
Original license and attribution files remain. The code and test fixtures come from that commit.
Upstream CI workflows are omitted. This snapshot adds the environment instructions and dependency lock.


Tested on Linux with CPython 3.11.16. Start from a fresh checkout of this commit:

```sh
uv venv --python 3.11.16 .venv
export PATH="$PWD/.venv/bin:$PATH"
export PYTHONUTF8=1 PYTHONDONTWRITEBYTECODE=1 PYTEST_DISABLE_PLUGIN_AUTOLOAD=1 AWS_EC2_METADATA_DISABLED=true
mkdir -p .venv/tmp
export TMPDIR="$PWD/.venv/tmp"
uv pip install --python .venv/bin/python -r requirements-domain.lock
uv pip install --python .venv/bin/python --no-build-isolation --no-deps -e .
.venv/bin/python -B -m pytest -q -o addopts= -p no:cacheprovider tests/test_parsers.py tests/test_readers.py tests/test_feed.py tests/test_writers.py
```

The selected tests exercise local fixtures or in-process clients. Restore by making a fresh checkout.
