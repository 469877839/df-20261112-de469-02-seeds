# Local data-engineering evaluation baseline

Upstream: https://github.com/opensearch-project/opensearch-py/commit/5952dc1ae35b0ce65c8e8169e82662d5d8504ce2
Original license and attribution files remain. The code and test fixtures come from that commit.
Upstream CI workflows are omitted. This snapshot adds the environment instructions and dependency lock.
Four unrelated authentication, TLS and notification test files with inline credential fixtures are omitted.


Tested on Linux with CPython 3.13.15. Start from a fresh checkout of this commit:

```sh
uv venv --python 3.13.15 .venv
export PATH="$PWD/.venv/bin:$PATH"
export PYTHONUTF8=1 PYTHONDONTWRITEBYTECODE=1 PYTEST_DISABLE_PLUGIN_AUTOLOAD=1 AWS_EC2_METADATA_DISABLED=true
mkdir -p .venv/tmp
export TMPDIR="$PWD/.venv/tmp"
uv pip install --python .venv/bin/python -r requirements-domain.lock
uv pip install --python .venv/bin/python --no-build-isolation --no-deps -e .
.venv/bin/python -B -m pytest -q -o addopts= -p no:cacheprovider -p pytest_mock.plugin test_opensearchpy/test_helpers/test_actions.py test_opensearchpy/test_helpers/test_search.py test_opensearchpy/test_helpers/test_result.py test_opensearchpy/test_serializer.py
```

The selected tests exercise local fixtures or in-process clients. Restore by making a fresh checkout.
