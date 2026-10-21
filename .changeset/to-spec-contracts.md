---
"mattpocock-skills": minor
---

`to-spec` writes a Contracts section: the types the spec introduces or changes (defined before anything references them, existing types referred to by name), each endpoint's method, path, auth, params, query, body and every response, and each function or method's signature and docstring with no body. Docstrings follow the project's documented convention, falling back to `@params` / `@return` / description. A changed contract appears in full, marked changed.
