# Example: Sample Intent — RealizeOS 5

> This is an example of a completed intent file, ready for execution.

## Goal

Add a caching layer to the LLM routing system to avoid re-computing optimal model selection for identical query patterns.

## Context

The LLM router currently evaluates routing rules on every request. For repeated query patterns (same skill, same complexity tier), the routing decision is deterministic and can be cached. This would reduce latency by 30-60% for common operations.

## Scope

### IN

- Add caching to `realize_core/optimizer/llm_router.py`
- Cache key: skill_name + complexity_tier + available_models hash
- TTL: 5 minutes (configurable)
- Cache invalidation when model config changes
- Tests for cache hit, cache miss, cache invalidation

### OUT

- Don't change the routing algorithm itself
- Don't modify the model configuration system
- Don't add external cache dependencies (use in-memory)

## Acceptance Criteria

- [ ] Repeated identical routing queries return cached result
- [ ] Cache TTL is configurable via environment variable
- [ ] Cache is invalidated when model configuration changes
- [ ] No stale routing decisions served (correctness over speed)
- [ ] Response latency for cache hits < 5ms
- [ ] All existing routing tests still pass
- [ ] New tests cover hit, miss, invalidation, and TTL expiry

## Constraints

- Must use in-memory caching only (no Redis dependency for this)
- Must not increase memory usage by more than 10MB
- Must follow existing async patterns

## Build Mode

**Mode:** `iterative`

> Trying multiple caching strategies (LRU, TTL-based, hybrid) to find the best one.

## Additional Notes

- Existing routing code: `realize_core/optimizer/llm_router.py`
- Experiment tracking: `realize_core/optimizer/experiment_engine.py`
- Reference the caching patterns in project-context.md
