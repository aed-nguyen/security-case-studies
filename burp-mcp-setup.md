# Burp MCP Setup

I connected Burp Suite Community 2026.7.3 to Codex with PortSwigger's MCP Server 1.3.0. The setup uses PortSwigger's packaged standard-input/output proxy.

## Failure

The packaged proxy process was being killed with exit code 137 before it could finish the connection. The configuration was pointing at the bundled Java runtime, so I tested the proxy separately from Burp and Codex to narrow down which process was failing.

## Fix

I ran the proxy with OpenJDK 21 and kept the same packaged proxy file. That removed the failing runtime from the path without replacing PortSwigger's proxy.

## Results

- Burp's MCP extension started normally.
- The standard-input/output handshake completed.
- Codex reported `Connected. 24 tools available.`
- The connection used Burp's current packaged proxy.

This gave me a repeatable way to keep the browser proxy, request history, and manual testing in Burp while using Codex to inspect the same testing session.
