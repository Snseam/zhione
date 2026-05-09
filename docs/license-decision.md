# License Decision

ZhiOne uses the Apache License 2.0.

This is a product and architecture decision, not legal advice.

## Recommendation

Use `Apache-2.0` as the repository license.

Why:

- ZhiOne is intended to become infrastructure for agent memory and local context
  systems, not only a small script.
- The license is permissive and friendly to commercial, academic, and private
  use.
- It includes an express patent grant, which is useful for infrastructure that
  may later include indexing, memory operation, or agent integration mechanisms.
- It requires preservation of license notices and notices when redistributed.
- It does not force downstream users to open-source larger systems that integrate
  with ZhiOne.

## Alternatives Considered

### MIT

MIT is simpler and very common. It is a good choice for small libraries and
tools, but it does not include the same explicit patent grant. For an agent
memory kernel that may become infrastructure, Apache-2.0 is a stronger default.

### GPLv3

GPLv3 protects openness more strongly, but it can discourage integration into
commercial tools, internal agent systems, and mixed-license desktop workflows.
That conflicts with ZhiOne's goal of becoming a broadly usable local memory
layer.

### AGPLv3

AGPLv3 is useful when the main risk is a hosted service taking the project
private. ZhiOne's initial direction is local-first infrastructure, so the
network-service trigger is not the most important constraint right now.

### Dual License

Dual licensing can be revisited if the project later needs a commercial license
or a separate documentation license. It adds governance complexity too early for
the current stage.

## Practical Rules

- Keep the full Apache-2.0 text in `LICENSE`.
- Keep `NOTICE` short unless third-party notices require expansion.
- Add `SPDX-License-Identifier: Apache-2.0` to source files once implementation
  starts.
- Treat raw personal notes and transcripts as content that needs explicit public
  review before committing, regardless of repository license.

## References

- Apache Software Foundation license page:
  <https://www.apache.org/licenses/LICENSE-2.0.html>
- SPDX Apache-2.0 identifier:
  <https://spdx.org/licenses/Apache-2.0.html>
- Choose a License overview:
  <https://choosealicense.com/licenses/>
