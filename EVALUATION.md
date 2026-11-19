# Lab 4 Evaluation: Testing, CI/CD, and Deployment

## 1. Unit Testing (`campus_agent/test_agent.py`)
- **Prompt / Approach:** Generated unit tests from the Lab 4 specification to test `get_campus_resource_info` (`test_library_lookup`, `test_tutoring_alias`, `test_case_insensitive`, `test_resource_not_found`, and `test_list_all_resources`).
- **What It Tests:** Verifies exact lookup (`"library"`), alias matching (`"tutoring"` -> `"Tutoring Center"`), case-insensitivity (`"LIBRARY"`), missing-resource error handling (`"dental office"` returning `"Available resources"`), and listing all resources (`"all"` returning at least 4 items).
- **Verification:** Installed `pytest` in the `ai-agents` conda environment and ran `python -m pytest test_agent.py -v` from `campus_agent`; all 5 tests passed.
- **Changes Made & Why:** Added `sys.path` adjustment so `agent.py` imports cleanly regardless of working directory, and added a `__main__` block so `python test_agent.py` also runs directly.

## 2. CI/CD Workflow (`.github/workflows/test.yml`)
- **Prompt / Approach:** Created a GitHub Actions workflow triggered on `push` to `main` using `ubuntu-latest` and Python `3.11`.
- **What It Does:** Checks out the repository, sets up Python 3.11, installs dependencies, and runs `pytest` inside `campus_agent`.
- **Changes Made & Why:** Added `google-adk` alongside `pytest` during dependency installation because importing `agent.py` evaluates ADK imports and `root_agent`, which would fail with `ModuleNotFoundError` in a clean runner otherwise. Set `working-directory: campus_agent` so `campus_resources.json` is found cleanly.

## 3. Code Quality & SlopCodeBench Reflection
- **AI vs. Manual Code:** The generated tests and workflow directly cover the lab requirements, though runtime/environment details (like `sys.path` and installing `google-adk` in CI) required careful checking rather than blind trust.
- **Erosion & Verbosity Observed:** In `appupdated.py`, there was an unused `import time` line and a session state key mismatch (`"response.cache"` vs. `"response_cache"`) that prevents caching from persisting across reruns.
- **Future Prompting Improvements:** State exact file paths, target versions, edge cases, and constraints up front, then run and inspect the code immediately to catch subtle bugs.

## 4. Impact on Students at JCC
- **Outcome & Value:** Gives Jamestown Community College (JCC) students a quick, conversational way to look up accurate hours, locations, contact details, and links for campus resources (such as the Library, Tutoring Center, Academic Advising, and Financial Aid) without digging through multiple web pages, supported by automated tests and continuous deployment.