# WebSocket API Design

## Use WebSocket When

- The client needs low-latency server push.
- The interaction is session-oriented or conversational.
- The system benefits from bidirectional messaging over a long-lived connection.
- Polling or webhook callbacks would be materially worse for latency or UX.

Do not use WebSocket for ordinary CRUD just because it feels modern.

## Start With Connection Semantics

Define the lifecycle before the message catalog:

- How clients authenticate during connect or immediately after connect
- How sessions are renewed or re-authenticated
- Whether one user may hold several concurrent connections
- How the server detects liveness and stale clients
- What happens on reconnect

Be explicit about:

- Heartbeats or ping-pong behavior
- Idle timeout
- Max session duration
- Reconnect backoff guidance

## Use A Stable Message Envelope

Wrap messages in a predictable envelope instead of sending anonymous JSON blobs.

Typical fields:

- `type`
- `id` or correlation ID
- `timestamp`
- `payload`
- `version`

Example:

```json
{
  "type": "chat.message.send",
  "id": "msg_123",
  "timestamp": "2026-03-11T14:25:00Z",
  "payload": {
    "room_id": "room_7",
    "text": "hello"
  },
  "version": 1
}
```

## Separate Commands From Events

- Use commands for client-to-server intent.
- Use events for server-to-client facts.
- Name them differently so direction and responsibility stay clear.

Example:

- Command: `order.cancel.requested`
- Event: `order.cancelled`
- Event: `order.cancel.rejected`

This separation makes retries, auditing, and authorization clearer.

## Define Delivery Guarantees

State what the client may rely on:

- At-most-once, at-least-once, or best-effort delivery
- Ordering guarantees per connection, stream, or entity
- Duplicate message handling expectations
- Ack or receipt patterns when reliability matters

If messages can be replayed or duplicated, require idempotent client handling.

## Handle Backpressure And Fan-Out

- Define server behavior when the client cannot keep up.
- Decide whether to buffer, drop, coalesce, or disconnect.
- Publish max message size and rate expectations.
- Distinguish personal events from broadcast or room events.

For high-volume streams, consider:

- Sequence numbers
- Resume tokens
- Snapshot plus delta patterns

## Design Errors And Close Behavior

Use explicit error messages for in-session failures and document close behavior for terminal failures.

Define:

- Error message shape
- Retryable versus fatal errors
- Close reasons and codes
- Whether the client should reconnect automatically

Example error:

```json
{
  "type": "error",
  "id": "msg_123",
  "payload": {
    "code": "ROOM_NOT_JOINED",
    "status": "FORBIDDEN",
    "message": "Join the room before sending messages.",
    "retryable": false
  }
}
```

Use `UPPER_SNAKE_CASE` for stable error codes in message payloads, and keep the core payload fields as `code`, `status`, and `message`. Add delivery-specific fields such as `retryable` only when the client genuinely needs them.

## Design Auth And Authorization

- Authenticate the connection and authorize each sensitive action.
- Re-check permissions for room joins, subscriptions, and commands that affect state.
- Avoid assuming that an authenticated socket is allowed to do everything.
- Define tenancy and scope boundaries clearly.

## Plan Evolution

- Version the envelope or message families when needed.
- Prefer additive message fields.
- Keep unknown fields safe to ignore.
- Deprecate message types before removal when clients may still emit them.

## Connect WebSocket To The Rest Of The System

- Decide whether WebSocket is the system of record or just a delivery channel.
- If REST or GraphQL also exist, keep write ownership explicit.
- Use WebSocket for live updates and session commands, not as a second, undocumented CRUD API.

## Review Checklist

- Is WebSocket justified by latency or bidirectional needs?
- Are connection lifecycle and reconnect semantics explicit?
- Do message envelopes separate commands, events, and errors cleanly?
- Are delivery guarantees and ordering documented?
- Is backpressure behavior defined?
- Are auth, authorization, and evolution rules clear?
