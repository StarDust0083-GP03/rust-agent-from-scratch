# Rust Agent From Scratch

A dependency-free Rust project for building a terminal AI agent, from first principles.

## If you get lost

Build one small, working layer at a time. Start with a program that reads a line and prints it back; then add the model API, conversation history, tools, and the terminal interface. Keep each step runnable, and avoid adding dependencies until the standard library is no longer enough.

Suggested order:

1. Read input and print a response.
2. Send one request to an AI provider and display its reply.
3. Preserve conversation history.
4. Add tool calls and execute one safe local tool.
5. Add streaming and a polished terminal UI.

The project currently contains only its Cargo manifest. No application code or third-party crates have been added.
