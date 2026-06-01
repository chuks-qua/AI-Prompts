# Example brief

A filled brief in the target style. The task: add a token-bucket rate limiter to an HTTP client. Use this as a shape reference, not a template to copy verbatim.

---

Add a token-bucket rate limiter to the HTTP client so callers can cap outbound request rate without changing call sites.

Expected outcomes
1. Rate limiting
   - The client admits at most `rate` requests per second on average, with short bursts up to `burst`.
   - A request that would exceed the limit waits until a token is available, then proceeds.
   - Burst means the maximum number of tokens the bucket holds, so a number of requests up to `burst` can fire back to back after an idle period.
   - Tokens refill continuously at `rate` per second, not in discrete ticks.
2. Configuration
   - `rate` and `burst` are set once when the client is constructed.
   - A `rate` of zero or less disables limiting; the client admits every request immediately.
3. Concurrency
   - The limiter is safe to call from multiple goroutines or threads at once.
   - Concurrent waiters are admitted in first-come order.
4. Cancellation
   - A request whose context or deadline is cancelled while waiting returns the cancellation error and consumes no token.

Implementation notes
- The bucket data structure, lock choice, and refill algorithm are implementation details.
- The public constructor signature and the existing `Do(request)` method signature must not change. Limiting happens inside the existing request path.
- Keep the implementation focused on the behaviours above. Do not add per-host limits or metrics.

Acceptance criteria
- [ ] Firing `burst + 5` requests against a `rate=10, burst=5` client takes at least `(burst + 5 - burst) / rate` seconds in total.
- [ ] After idling longer than `burst / rate` seconds, the next `burst` requests proceed without measurable delay.
- [ ] A `rate <= 0` client returns from 1000 sequential requests with no added latency.
- [ ] Cancelling a waiting request's context returns the context error and leaves the token count unchanged.
- [ ] Running the limiter under a race detector with concurrent callers reports no data race.

Context (why)
A downstream API began returning 429s under load. Capping outbound rate at the client keeps every call site unchanged and removes the need for scattered manual sleeps.
