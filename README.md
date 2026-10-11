# Bug triage

A Cupboard trigger pack. It reads messages in the chat channels you pick, decides which ones report
a bug, and files each bug as a ticket in the project you pick.

It names no provider. It binds to the `chat-message` datatype, so any installed chat provider
(the `slack` package today) can feed it. It files through `ticket:file`, so any installed ticket
provider that serves `ticket.create` and `ticket.containers` (the `asana` package today) can
receive the tickets.

## Setup

1. Install a chat provider and a ticket provider from the Cupboard and connect both on
   Settings → Integrations.
2. Install this pack. Its section on the same page asks which project it runs in, then for:
   - **Channels to watch**: filled from the chat provider's `chat.channels`.
   - **File bugs in**: filled from the ticket provider's `ticket.containers { purpose: 'create' }`,
     which lists only the projects a ticket can actually be filed in.
3. Review the disarmed binding and arm it.

## Guarantees

- The worker supplies only a title and the observed/expected text. The server takes the
  destination from the installation, so the message cannot redirect a ticket.
- A message from a channel the installation does not watch cannot file.
- The ticket body quotes the message as untrusted evidence.
- Each trigger run files at most once: the attempt is recorded before any provider call, and a
  replay recovers the recorded ticket.

## License

MIT.
